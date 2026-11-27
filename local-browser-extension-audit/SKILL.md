---
name: local-browser-extension-audit
description: Security-audit a browser extension (Chrome MV3 / Firefox MV2-MV3) for bounty hunting — manifest and permission abuse, content-script injection, storage exfiltration, CSP bypass, webRequest/declarativeNetRequest abuse, supply-chain compromise, extension fingerprinting, and WASM-core analysis. Use when auditing an extension's source or installed package, triaging an extension as a bounty target, or proving an extension vulnerability with a PoC. Built for Sky's FREE-only, PoC-proven bounty hunt; includes the 1Password round-40 context (extension 8.12.46.29, WASM core, webAuthnValidateRelyingParty).
---

# Browser Extension Security Audit

## When to use

- Auditing a browser extension as a bug-bounty target (source, unpacked `.crx`/`.xpi`, or installed profile copy).
- Triaging whether an extension is worth a full audit round: permission scope vs stated purpose, update history, supply-chain signals.
- Proving a concrete extension vulnerability (data theft, session hijack, origin confusion, privilege escalation) with a reproducible PoC.
- Reviewing your own extension before submission so it does not ship the same holes you hunt in others.

## When NOT to use

- Building a new extension from scratch — use `browser-extension` (dev-oriented scaffold skill).
- Auditing a web app that merely happens to run in a browser — use the web-hunt skills (`local-0xsteph-web-hunter`, `hunt-dom`, `hunt-xss`).
- Auditing a native mobile app — use `mobile-security`.

## Core rule (from the hunt)

A model/simulation PoC never establishes impact. Only execution against the real
artifact does: the real extension package, the real deployed build, the real
browser. A finding is submittable only when it is FREE-route, PoC-proven, and
checked against known issues / prior audits / the live deployed state first.
Never submit a known or fixed finding.

## Phase 0 — Get the real artifact

1. **Identify the exact build.** Record extension ID, version, store URL, and
   install source (Chrome Web Store / AMO / Edge Add-ons / sideloaded). Version
   matters: a finding on 8.12.46.29 may already be fixed in 8.12.47.
2. **Extract the package — never audit the store page description.**
   - Chrome: copy the version folder from the profile
     (`.../Chrome/User Data/Default/Extensions/<id>/<version>/`), or download
     the `.crx` and unzip it (`unzip ext.crx` after stripping the CRX header,
     or use `crx3` tooling).
   - Firefox: `.xpi` is a zip. `unzip ext.xpi -d ext/`.
   - Verify the hash of what you extracted and record it; every later claim
     cites this exact build.
3. **Inventory the package.** List every file, note sizes, and flag:
   - minified/bundled JS with no source map (hides logic),
   - `.wasm` files (opaque native core — see Phase 5),
   - remote URLs baked into code (CDN scripts, analytics, update endpoints),
   - `manifest.json` as the first read (Phase 1).

## Phase 1 — Manifest & permission audit

Read `manifest.json` in full before any JS. Score every entry against the
extension's stated single purpose.

### Permission red flags (Critical/High unless justified)

| Manifest entry | Why it is dangerous |
|---|---|
| `"<all_urls>"` / `*://*/*` in `host_permissions` | Read/write every page the user visits |
| `cookies`, `webRequest`, `webRequestBlocking`, `debugger` | Session theft, traffic interception, full DevTools control |
| `tabs` + `history` + `topSites` | Full browsing-profile reconstruction |
| `scripting` + broad hosts | Arbitrary code execution on any page |
| `nativeMessaging` | Bridge out of the browser sandbox to a host binary |
| `management` | Can disable/uninstall other extensions (incl. security ones) |
| `web_accessible_resources` with `"matches": ["<all_urls>"]` | Any site can detect/fingerprint the extension and probe its resources |
| Missing `content_security_policy` or `unsafe-eval` / `unsafe-inline` | Remote-code execution via injected script |
| `externally_connectable` with `"matches": ["*://*/*"]` | Any website can message the extension |

### Checks

- Every permission must map to a named feature. An unexplained permission is
  a finding candidate, not a style nit.
- `optional_permissions` requested at install time (not on user action) is a
  smell — the extension grabs scope before the user can judge the feature.
- Compare the manifest against the previous published version (store
  version history / CRX history mirrors). A permission added silently in an
  update is a classic supply-chain takeover signal.
- `update_url` pointing somewhere other than the official store is a
  Critical supply-chain flag.

## Phase 2 — Content-script injection audit

Content scripts run in the user's pages. Audit every entry in
`content_scripts` and every `chrome.scripting.executeScript` / `tabs.executeScript` call.

1. **Match patterns.** `matches: ["<all_urls>"]` + `run_at: document_start` +
   DOM write access = the extension can read passwords, tokens, and form data
   on every site. Ask: does the feature need this?
2. **Isolated world vs MAIN world.** Default is isolated — good. `world: "MAIN"`
   bridges (via `window.postMessage`) must validate origin AND payload shape;
   an unvalidated `postMessage` listener is a page-to-extension injection
   (any malicious site can drive the extension).
3. **DOM sinks.** Grep for sinks in content scripts:
   ```
   rg -n "innerHTML|outerHTML|document\.write|insertAdjacentHTML|eval\(|new Function\(|setTimeout\(\s*['\"]|setInterval\(\s*['\"]" <ext>/
   ```
   Any sink fed by page data, storage, or a message is an XSS hole *inside*
   the extension's privileged context — it executes with the extension's host
   permissions, not the page's.
4. **Message validation.** Every `chrome.runtime.onMessage` listener must
   validate `sender` (origin/tab) and the message schema (type + fields).
   A listener that acts on `message.url`, `message.code`, or `message.html`
   without validation is a confused-deputy: any web page that can reach the
   extension can make it fetch, eval, or render attacker input.
5. **Prototype pollution / object merge.** `Object.assign` / deep-merge of
   message payloads into extension state is a classic privilege-escalation
   primitive — check merge targets and whether `__proto__` is reachable.

## Phase 3 — Storage & exfiltration audit

1. **What leaves the machine?** Grep every network egress:
   ```
   rg -n "fetch\(|XMLHttpRequest|WebSocket|navigator\.sendBeacon|chrome\.runtime\.sendMessage" <ext>/ --type js
   ```
   For each call record: destination host, data sent, trigger. Data sent to a
   host not named in the privacy policy = exfiltration candidate.
2. **Sensitive storage.** Secrets (tokens, vault items, seeds) in
   `chrome.storage.sync` are synced in plaintext-ish form to the vendor's
   cloud; in `localStorage` on a host page they are readable by page JS.
   Correct homes: `chrome.storage.local` (device-local) or encrypted-at-rest
   storage with a key the extension never ships.
3. **Clipboard / autofill paths.** Extensions that read the clipboard or
   autofill credentials must never log, cache, or POST that content. Grep
   for `clipboardRead`, `navigator.clipboard`, and any autofill fill routine
   that copies values into page DOM (page JS can then read them back).
4. **Analytics & crash beacons.** A `sendBeacon` on every page load with the
   full URL is browsing-history exfiltration even when the vendor calls it
   "telemetry". Check opt-out: is it on by default?

## Phase 4 — Network-interception audit

1. **MV3 `declarativeNetRequest` rulesets.** Read every ruleset JSON. Rules
   that redirect, strip headers (`Authorization`, `Cookie`, CSP), or rewrite
   responses on banking / SSO / wallet domains are Critical.
2. **MV2 `webRequest` blocking listeners.** Same test, plus: can a content
   script or page message add/remove rules at runtime
   (`updateDynamicRules`)? Runtime rule mutation driven by page input is
   request hijacking.
3. **Header stripping.** Look for removal of `Content-Security-Policy`,
   `X-Frame-Options`, `Strict-Transport-Security`, or `Set-Cookie` flags —
   each one downgrades the security of every site the user visits.
4. **Update & remote-code checks.** MV3 forbids remote code, but check
   anyway: `importScripts` of a remote URL in the service worker, WASM
   fetched from a CDN at runtime, config JSON fetched and `eval`'d, or a
   "feature flag" endpoint that can toggle code paths. A fetched config that
   changes *behavior* (not just strings) is a remote-code vector.

## Phase 5 — WASM / native-core audit

Many password managers and wallets ship a WASM core (e.g. 1Password
op-core). The JS glue is the audit surface; the WASM is the vault.

1. **Glue-import inventory.** Extract the import object from the JS glue
   (often in `background.js`). Each `__wbindgen_*` import is a host
   capability the WASM can call. Flag: `__wbindgen_free` on *input* strings
   (double-free trap — the input is Rust-owned after the call; freeing it
   traps and bricks later calls), `__wbindgen_cast_*` stubs assumed to be
   identity (some are UTF-8 readers / BigInt converters — verify each from
   the glue, never assume).
2. **Never free what you do not own.** Free only multivalue results. A
   double-free in the glue is a reliable extension-wide DoS (every later
   WASM call fails).
3. **Async exports.** An async WASM export needs the faithful
   `__wbg_new_typed_*` promise adapter and `__wbg_queueMicrotask_*`; without
   them it returns 0 or never settles. An export that parks forever because
   the core needs a host transport (`chrome.runtime`, IndexedDB) is
   UNVERIFIED-blocked, not killed — record it as such.
4. **Multiple modules, same helper names.** Glue files can embed several
   WASM modules' import objects with same-hash helper names — always extract
   definitions from the *target module's* region (e.g. the op-core NHA
   region), not the first occurrence in the file.
5. **Boundary checks on the WASM API.** Every exported function that takes a
   user-controlled string/buffer (origin, relying-party ID, vault item) must
   be checked for: origin/RP validation (substring match is NOT validation),
   length limits, and null/empty handling. See the WebAuthn note below.

## Phase 6 — WebAuthn / passkey surface (1Password round-40 context)

Real target context: 1Password extension 8.12.46.29, WASM core, export
`webAuthnValidateRelyingParty`. Audit any extension that touches passkeys
or WebAuthn ceremonies:

1. **Relying-party validation.** `webAuthnValidateRelyingParty` (or its
   equivalent) must compare the RP ID against the origin by exact/suffix
   rules from the WebAuthn spec — a substring check (`origin.includes(rpId)`)
   lets `evil-1password.com` pass for `1password.com`.
2. **Ceremony binding.** Challenge must be server-issued, single-use, and
   bound to the session; origin and RP ID must be checked on the *response*,
   not just the request. A cryptographically valid assertion that seats a
   session for a different user than the credential is bound to is Critical.
3. **Counter / clone detection.** A signature-counter regression must be
   surfaced, not silently accepted — it hides a cloned authenticator.
4. **Live-browser test only.** WebAuthn flows need a real browser with a
   real authenticator path; static reading can mark candidates but cannot
   prove them. If the live test needs Sky's browser or his 1Password
   account, that is a Sky-only step — record it, do not fake it.

## Phase 7 — Supply-chain & fingerprinting audit

1. **Version-diff the last 3 releases.** New permissions, new hosts, new
   analytics SDKs, minified code where source was previously readable, or a
   maintainer/ownership change = investigate before anything else. Most
   extension compromises arrive as an "update", not as original malware.
2. **Dependency audit.** Check bundled third-party libs against known
   vulnerable versions; a known-vulnerable bundled lib in a privileged
   context is a finding.
3. **Fingerprinting.** `web_accessible_resources` + a distinctive DOM marker
   or `chrome.runtime.id` exposure lets any site detect the extension and
   tailor phishing ("we see you use 1Password — re-authenticate here").
   Fingerprinting alone is Low; fingerprinting that enables targeted
   credential phishing on the extension's own user base is Medium+.
4. **Store-metadata check.** Compare the store listing (screenshots,
   description, privacy policy) against the manifest. A "single purpose"
   mismatch or a privacy policy that omits a data flow you found in code is
   a store-policy violation and strengthens the report.

## Phase 8 — PoC & severity

1. **Prove with the real artifact.** Load the exact extracted build in a
   clean browser profile. Drive the vulnerable path end-to-end (malicious
   page → extension → exfiltrated data / forged action). Screenshots, HAR,
   and console logs are the evidence; a code quote alone is not a PoC.
2. **Severity = blast radius x reach.**
   - Critical: reads credentials/tokens/secrets from arbitrary sites, or
     executes code with extension privileges from a web page.
   - High: exfiltrates browsing data / page content to a third party, or
     bypasses origin checks on a security boundary (WebAuthn RP, vault unlock).
   - Medium: over-broad permissions with no demonstrated abuse, fingerprinting
     enabling targeted phishing, stale/vulnerable bundled dependency.
   - Low: missing best practice with no reachable abuse path.
3. **Pre-submit gate (mandatory).** Before drafting: check the program's
   known-issues list, prior audits, public disclosures, and the live
   deployed state at the pinned version. Check the submission route is
   current (email/portal, cost, rep minimum, KYC, PoC requirement) and the
   program is active and pays cash. Never submit a known/fixed finding.

## Tooling

- `unzip`, `rg` (ripgrep), `strings`, `jq` (manifest parsing),
  `python3 -c "import json..."` (manifest diffing across versions).
- `wasm2wat` / `wasm-objdump` (WASM core inventory), `node` (run the JS
  glue in a harness with stubbed `chrome.*` APIs).
- A clean Chromium/Firefox profile for live PoC; `chrome://extensions`
  developer mode to load the unpacked build.
- CRX history mirrors / store version history for supply-chain diffs.

## Output format

```markdown
# Extension audit: <name> <version> (<id>)
- Artifact hash: <sha256 of extracted package>
- Verdict: <SUBMITTABLE / KILLED / UNVERIFIED> + one-line reason
- Findings table: ID | severity | title | evidence (file:line / PoC step) | blast radius
- Per finding: mechanism, PoC steps (reproducible), impact, fix
- Gates: program, route, cost, rep min, KYC, PoC req, known-issue check
```

## References

- Chrome Extensions docs — developer.chrome.com/docs/extensions (MV3,
  permissions, declarativeNetRequest, CSP).
- Firefox Extension Workshop — extensionworkshop.com (MV2/MV3, AMO review).
- OWASP — Web Security Testing Guide, section on browser extensions.
- Chrome Web Store program policies — single purpose, remote code, user data.
- WebAuthn Level 3 (W3C) — RP ID validation, ceremony verification.
