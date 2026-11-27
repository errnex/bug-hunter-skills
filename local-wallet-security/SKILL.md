---
name: local-wallet-security
description: Audit a crypto wallet and its user's exposure for bounty hunting and self-defense — seed/private-key handling in wallet code, blind signing, unlimited approvals, Permit/Permit2, setApprovalForAll, EIP-7702 delegation, malicious dApps, transaction simulation, approval phishing, and on-chain wallet privacy/traceability (address clustering, ENS, exchange links). Use when reviewing a signature or approval request before signing, auditing a wallet's standing approvals, auditing wallet source code, or scoring how private/traceable a wallet is. Replaces the thin wallet-privacy wrapper with a full methodology. Built for Sky's FREE-only, PoC-proven bounty hunt.
---

# Wallet Security & Privacy

## When to use

- Someone pastes a signature / transaction request and asks "is this safe to sign?"
- Auditing a wallet's standing approvals (allowances, operators, delegations).
- Auditing wallet source code (extension, mobile, hardware firmware, signer lib)
  for key-material vulnerabilities.
- Scoring how private or traceable an EVM wallet is (privacy scan + fix plan).
- Triaging a suspected drainer / phishing site before interacting.
- Hunting wallet bugs for bounty: seed handling, signing, simulation, approvals.

## When NOT to use

- General smart-contract auditing — use the `evm-audit-*` / `solidity-auditor`
  skills. This skill is about the *wallet* and the *signing* surface, not the
  protocol the wallet talks to.
- Checking a token for honeypot/rug — use the no-key screening stack
  (GoPlus, Honeypot.is, Sourcify, DexScreener).

## Hard safety rules

- **Never ask for a seed phrase, private key, or password.** If someone pastes
  one, do not repeat it: tell them it has now passed through this chat, treat
  it as leaked, and move funds to a fresh wallet immediately.
- **Never ask for an API key in chat.** Keys belong in the tool's settings or
  the user's own shell profile; a pasted key must be rotated.
- **Never invent on-chain state.** If you need the approval list, the request
  payload, or the wallet's history, ask for it — never invent allowances,
  spenders, or balances.
- A wrong "safe to sign" answer can cost everything in one click. When in
  doubt, the verdict is DON'T SIGN until the unknown is resolved.

## Part A — Live signature / approval review (sign / don't-sign)

### A1. Scope first

1. **What is being reviewed?** (a) a LIVE request about to be signed,
   (b) an existing wallet's approvals, (c) a suspicious site/dApp/message.
2. **Which chain?** Approvals are per-chain — Base is not mainnet.
3. **Which signer?** Software (MetaMask, Rabby, Phantom) vs hardware
   (Ledger, Trezor, Keystone). Hardware lowers key-theft risk but does NOT
   stop signing a malicious approval. Note whether clear signing is on.
4. **Get the artifact.** The raw request payload (`method`, `params`,
   typed-data `domain`/`message`, `to`/`data` calldata), the approval list
   (revoke.cash / Etherscan Token Approval Checker export), and/or the site
   URL + how the user arrived (Discord DM? X link? Google ad?).

### A2. Severity model — blast radius x likelihood

| Severity | Meaning |
|---|---|
| Critical | Can drain everything now, or the wallet is likely already compromised. Any `eth_sign` blind-sign of an unknown payload, a Permit2 full-balance sig to a hostile spender, an unknown EIP-7702 delegation. Act immediately. |
| High | Large standing exposure: unlimited ERC-20 allowance or `setApprovalForAll` to an unverified/unknown spender. |
| Medium | Real but bounded: finite allowance larger than needed, stale approval to a known-but-dormant contract, Permit2 allowance with sane limits. |
| Low | Tight, intentional, scoped grants to verified contracts; finite allowance ≈ amount being spent. |
| Info | Hygiene notes (disconnecting a dApp does NOT revoke; bookmark the real domain; enable clear signing). |

Blast radius = how much can be taken if this is malicious. Likelihood = how
plausibly hostile (unverified spender, phishing delivery, no simulation).

### A3. Signature & approval taxonomy — name the exact type before judging

1. **ERC-20 `approve(spender, amount)`** (`0x095ea7b3`). Grants `spender`
   the right to pull up to `amount` of one token, repeatedly. Unlimited =
   `uint256` max. Safe: finite cap ≈ amount being spent, to a verified,
   intended contract. Dangerous: unlimited cap, or any cap to an unverified
   spender.
2. **`increaseAllowance`** — same risk class as `approve`.
3. **Permit — EIP-2612.** Gasless off-chain signature authorizing a
   spender's allowance. No on-chain tx, no trace in history — invisible
   until the attacker pulls funds. Recognize: `signTypedData_v4` with
   `owner/spender/value/nonce/deadline`. Dangerous: unknown spender, or a
   far-future deadline.
4. **Permit2 (Uniswap universal approval).** One signature authorizes
   Permit2 to move your tokens to a spender — and can drain allowances
   across *every token ever approved to Permit2*. Highest-blast-radius
   signature in the ecosystem. Recognize: Permit2 domain, `PermitSingle` /
   `PermitBatch` / `PermitTransferFrom` with `details` (token, amount,
   expiration). Safe only on the protocol you deliberately chose, bounded
   amount, short expiration.
5. **`setApprovalForAll(operator, true)`** (`0xa22cb465`). Blanket control
   of an entire NFT collection. Safe: a major verified marketplace you are
   actively listing on. Dangerous: requested by a "mint"/"claim"/"airdrop"
   flow (a mint never needs operator rights over your existing NFTs).
6. **`eth_sign` — raw-hash blind signature.** Signs an arbitrary 32-byte
   hash with no human-readable context. Blast radius: unbounded.
   **Verdict: NEVER sign `eth_sign`.** No legitimate modern dApp needs it.
7. **`personal_sign` / `signTypedData` — disguised intents.** Normally
   benign (login / Sign-In with Ethereum) — but attackers hide a permit,
   order, or approval inside the payload. Read the decoded message. A real
   login is human-readable; any `Permit`/`PermitSingle`/order struct,
   `spender`, `value`, or unreadable hex inside a "sign-in" is dangerous.
8. **EIP-7702 authorization / delegation.** Authorizes your EOA to delegate
   its code to a contract — the account then executes that contract's logic.
   A malicious delegate can auto-forward any incoming balance in the same
   block. >90% of observed 7702 delegations are malicious. **Unknown or
   unverified delegation target = treat the wallet as FULLY COMPROMISED**,
   not "just revoke it" — move assets to a fresh wallet. Legitimate 7702
   delegates to a verified, audited implementation you chose.

### A4. Standing-approval audit

Given an approval list, enumerate every active grant and flag:

- Unlimited allowances (uint256 max) — High (Critical if spender unverified).
- Unknown/unverified spender — Critical.
- Stale approvals to contracts no longer used — Medium, revoke (a "safe"
  contract can be exploited later).
- Every `setApprovalForAll` grant — flag any to a marketplace no longer used.
- Permit2 allowances — flag full-balance amounts or long expiries.
- EIP-7702 delegations — unknown target = compromised (fresh-wallet path).
- **Address poisoning** — dust / $0 transfers from a lookalike vanity
  address (matching first/last chars of a real address), planted so the
  victim later copies the wrong address from history. Never copy a
  destination from tx history; verify the full address.
- **Delayed draining** — drainer kits (Inferno/Pink-style) often wait days
  after the malicious signature before pulling funds. "Nothing happened
  yet" is NOT safety.

### A5. Transaction simulation (before signing)

- Simulate the exact calldata before signing: use the wallet's built-in
  simulation (Rabby, MetaMask), Tenderly simulation, or a local fork
  (`cast` / `anvil` + `eth_call` trace). Read the *state diff*, not the
  dApp's description: which tokens leave, to whom, what approvals change.
- If simulation is unavailable or the dApp blocks it, that is itself a red
  flag — do not sign blind.
- For bounty PoCs: a LiteSVM / fork run against the real deployed bytecode
  is the proof. A Python model of the same logic is not proof.

### A6. Verdict format

```
VERDICT: SIGN / DON'T SIGN / DON'T SIGN — COMPROMISED, MOVE FUNDS
Blast radius: <what can be taken>
Evidence: <exact fields from the payload — spender, amount, deadline, domain>
Remediation: <revoke at revoke.cash / fresh wallet / exact steps, in order>
```

## Part B — Wallet codebase audit (seed & key-material handling)

For auditing wallet source (extension, mobile, firmware, signer library):

1. **Seed generation.** Entropy source (CSPRNG, never `Math.random` or
   time-seeded PRNG), mnemonic generation (BIP-39 wordlist, checksum
   verified), derivation paths (BIP-44/49/84) — a weak or predictable
   entropy source is Critical.
2. **Seed storage.** Plaintext seed/private key in `localStorage`,
   `chrome.storage.sync`, logs, crash reports, or analytics payloads =
   Critical. Correct: OS keychain / encrypted vault with a user password
   (Argon2/scrypt KDF, never a fast hash), keys zeroized from memory after use.
3. **Seed exposure paths.** Clipboard copy without clearing, seed shown in
   DOM (page JS can read it back), seed included in export/backup files
   without encryption, seed echoed in error messages.
4. **Signing path.** Is the message shown to the user exactly what gets
   signed? Any transform between display and signing (truncation, re-encoding,
   domain substitution) is a blind-signing hole. `eth_sign` support in the
   wallet at all is a finding.
5. **dApp origin binding.** Does the wallet bind the signing request to the
   requesting origin, and show that origin unspoofably? A dApp that can
   present another origin's identity (iframe tricks, opener confusion) can
   phish approvals.
6. **Update / supply-chain.** Extension wallets auto-update; diff releases
   for new permissions, new hosts, new bundled libs (see
   `local-browser-extension-audit` Phase 7).
7. **Rule-pack method (dontrust-style).** Run the wallet rule pack / scanner
   first for coverage, then deep-read every hit to separate real findings
   from noise, and write a reviewed report — never submit scanner output raw.
   Codify a new incident as a rule (vulnerable + fixed revision) so the pack
   grows.

## Part C — Wallet privacy & traceability (replaces wallet-privacy)

The old `wallet-privacy` skill was a 13-line wrapper around one MCP scan.
This section is the full replacement: score traceability from public chain
data, then give a fix plan.

### C1. Privacy scan

From the wallet's public history (EVM chains), score 0 (exposed) to 100
(private). Check each linkage, with confidence (Likely / Possible / Weak)
and the evidence behind it — never state ownership as fact unless a public
label says so:

- **Exchange links** — deposits/withdrawals to a CEX (KYC'd endpoint).
- **ENS / naming** — ENS name, Lens handle, or any human-readable label
  tying the address to a person.
- **Counterparty clustering** — repeated counterparties, shared funding
  source, same deployer, or airdrop/faucet trails linking wallets.
- **Behavioral fingerprint** — distinctive amounts, timing, gas habits, or a
  unique token/collection set that identifies the owner across wallets.
- **Dust / poisoning** — incoming dust or lookalike-address transfers
  designed to link or misdirect.
- **Contract interactions** — approvals and dApp usage that reveal intent
  (e.g. a payroll contract, a specific DAO, a dating-app payment).

Report order: score + band, then the costliest findings with evidence, then
what was read. If the address is a contract, a service, or unused, say why
there is no score — never invent one.

### C2. Fix plan (when asked, or score < 80)

- **Today first** (with points won back): revoke stale approvals, move funds
  off the exposed address to a fresh wallet, stop reusing the address for
  new activity, remove or rotate the ENS link.
- **From now on:** one address per purpose (separate hot / cold / public),
  never fund the cold wallet from the hot one directly, use a fresh address
  per counterparty where it matters, verify full addresses (poisoning).
- **On chain for good:** past transactions cannot be deleted — say so
  plainly. Privacy going forward is compartmentalization, not erasure.
- Keep steps as written from the wallet's or name service's own help pages.

### C3. Privacy hygiene rules

- A seed phrase or private key pasted anywhere is leaked — move funds.
- "Disconnecting" a dApp does not revoke approvals and does not unlink history.
- A privacy score is a likelihood model, not an identity verdict.

## Part D — Bounty context (Sky's hunt)

- **FREE only.** A wallet/extension program is never called free until
  submission cost, rep minimum, KYC, and PoC requirement are all explicitly
  confirmed from the program page, sidebar, and structured data — plus the
  program is active/submittable and pays cash.
- **Pre-submit gate (mandatory).** Check known-issues lists, prior audits,
  public disclosures, and the live deployed state at the pinned version
  before drafting. Never submit a known/fixed finding.
- **PoC-proven only.** For extension/wallet findings, the PoC runs against
  the real build / real deployed bytecode (fork execution), with logs and
  artifacts. Simulation-only impact is not impact.
- **Known dead ends (do not re-audit without new facts):** Ticko without a
  new access code or published source; 1Password webAuthn export without a
  live-browser test (Sky-only); Exponent public source vs deployed binary
  divergence (disclose openly, do not re-audit the public source alone).

## Tooling

- `cast`, `anvil` (simulation / fork), `revoke.cash`, Etherscan Token
  Approval Checker (approval lists).
- `rg`, `strings`, `wasm2wat` (wallet/extension code audit).
- Block explorers + GoldRush/Covalent (balances, history) for privacy scans.
- Tenderly / local fork for pre-sign simulation.

## Output format

```markdown
# Wallet review: <address / request / codebase> <date>
- Verdict: <SIGN / DON'T SIGN / COMPROMISED — MOVE FUNDS / SUBMITTABLE / KILLED / UNVERIFIED>
- Score (privacy): <0–100 + band> (Part C only)
- Findings table: ID | severity | title | evidence | blast radius
- Per finding: mechanism, PoC steps, impact, fix
- Gates: program, route, cost, rep min, KYC, PoC req, known-issue check
```

## References

- EIP-2612 (Permit), EIP-7702 (delegation), ERC-20/721/1155 approval semantics.
- revoke.cash — approval hygiene; Etherscan Token Approval Checker.
- OWASP Mobile Top 10 / MASVS (mobile wallets); Chrome Extension docs
  (extension wallets — see `local-browser-extension-audit`).
- BIP-39 / BIP-32 / BIP-44 (seed & derivation); WebAuthn Level 3 (passkeys).
