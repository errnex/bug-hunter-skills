# Security Bounty Hunter

Use this when the goal is practical vulnerability discovery for responsible disclosure or bounty submission, not a broad best-practices review.

## When to Use

- Scanning a repository for exploitable vulnerabilities

- Preparing a Huntr, HackerOne, or similar bounty submission

- Triage where the question is 'does this actually pay?' rather than 'is this theoretically unsafe?'

## How It Works

Bias toward remotely reachable, user-controlled attack paths and throw away patterns that platforms routinely reject as informative or out of scope.

## In-Scope Patterns

These are the kinds of issues that consistently matter:

| Pattern | CWE | Typical impact |
| --- | --- | --- |
| SSRF through user-controlled URLs | CWE-918 | internal access, metadata theft |
| Auth bypass in middleware or API guards | CWE-287 | unauthorized account or data access |
| Remote deserialization or upload-to-RCE paths | CWE-502 | code execution |
| SQL injection in reachable endpoints | CWE-89 | data exfiltration, auth bypass, destruction |
| Command injection in request handlers | CWE-78 | code execution |
| Path traversal in file-serving paths | CWE-22 | arbitrary file read or write |
| Auto-triggered XSS | CWE-79 | session theft, admin compromise |

## Skip These

These are usually low-signal or out of bounty scope unless the program says otherwise:

- Local-only `pickle.loads`, `torch.load`, or equivalent with no remote path

- `eval()` or `exec()` in CLI-only tooling

- `shell=True` on fully hardcoded commands

- Missing security headers by themselves

- Generic rate-limiting complaints without exploit impact

- Self-XSS requiring the victim to paste code manually

- CI/CD injection that is not part of the target program scope

- Demo, example, or test-only code

## Workflow

1. Check scope and the four submission gates first: program rules, SECURITY.md, disclosure channel, exclusions, the stake or fee, the reputation minimum, the KYC requirement, and the PoC requirement.
2. Find real entrypoints: HTTP handlers, uploads, background jobs, webhooks, parsers, and integration endpoints.
3. Run static tooling where it helps, but treat it as triage input only.
4. Read the real code path end to end.
5. Prove user control reaches a meaningful sink.
6. Confirm exploitability and impact with the smallest safe PoC possible.
7. Check for duplicates before drafting a report.
8. Check the finding against known issues, prior audits, public disclosures, and the live deployed state at a pinned block, and verify the current submission route.
9. Prove impact by executing the proof of concept against the deployed bytecode when the finding depends on deployed behavior.

## Check submission economics first

- Verify the submission cost before you start deep audit work. Some Sherlock programs require a 250 USDC stake per report, returned when the report is valid and forfeited when it is invalid or a duplicate. An Immunefi fee can return as credit when the report is valid.

- Record the cost, the refund rule and the forfeit rule in your audit notes before you read code, and ask the user to confirm the cost is acceptable when a stake or fee applies.

- Check the current liquid funds against a stake before you commit audit hours to that program, and defer deep work on a program the user cannot afford to submit to.

- Treat a HackerOne submission as a spent slot. A submitted report consumes a trial or report slot even when the triager closes it as not applicable.

- Look for the fee in the program structured data as well as the page text, because a fee can hide in a structured field while the page text says nothing.

## Verify the four submission gates before deep work

- Record the submission cost, the reputation minimum, the KYC requirement, and the PoC requirement for the program before you start deep work.

- Read the program page sidebar and the page structured data or program JSON for each gate, and do not take a gate from the rewards section or the page text alone when the sidebar or structured data says something different.

- Treat a program as free only after all four gates are confirmed, and name the source that confirmed each gate.

- Check the platform gate as well as the program gate, because a platform gate can block an otherwise free program.

- Treat Bugcrowd as requiring Jumio identity verification before any Managed Bug Bounty program, based on the Bugcrowd changelog dated 2026-05-18.

- Treat YesWeHack as requiring MangoPay KYC and two credits per report, and HackerOne as requiring Veriff identity verification before any submission.

- Treat HackenProof gates as varying by program, and read the reputation minimum and submission cost from the program page itself.

- Recheck each platform baseline live when you return to a platform, because platform gates change and an old baseline goes stale. Bring a gate-blocked program to the user with the exact gate and cost instead of spending audit hours on it.

## Check the program is live and pays cash

- Confirm the program is active and submittable before you spend audit hours on it, and check the program state, the submission route, and whether it is paused.

- Confirm a cash reward exists before you spend audit hours on it, with a dollar amount in the brief or statistics, a non-empty reward summary or range, and a non-empty average payout.

- Check program freshness as well as state: read the page last-updated date and confirm the scoped code is still deployed and still holds value. A hacked target with a neutered repo and zero-value vaults is not an audit target. Bring a paused, disclosure-only, or zero-cash program to the user with the exact gate instead of auditing it.

## Check the finding is not already known or fixed

- Check whether the finding is already known or fixed before you draft any report: read the known-issues list, prior audits, and public disclosures.

- Verify the live deployed state at a pinned block before you call a finding live. A finding fixed in deployed code is not submittable even when it is real in an old commit.

- Verify the current submission route before you draft any report, and never submit a known or fixed finding. A finding named in the known-issues list is closed as known with no reward unless you find a genuinely new angle. Record each check in your audit notes with its source and pinned block.

## Prove impact against the deployed code

- Treat a model or simulation proof of concept as a hypothesis, never as proof of impact. A model can miss router rules, boundary checks, and tick assignment that change the outcome on chain.

- Prove impact by executing the proof of concept against the real deployed bytecode when the finding depends on deployed behavior. Fetch the deployed program and verify its hash against the deployed program data first, using a lightweight fork executor when a full validator does not fit the machine.

- Never quote an impact figure or a severity before the fork execution passes. A model-derived dollar figure is void when the execution reverts. Record the deployed program hash, the executor result, and the revert reason when execution fails.

## Prove the trigger is reachable

- Read the payout function and the invariant math before you rate a finding, and derive the trigger state from the actual pricing, not from headline arithmetic on gross values.

- Write a negative proof of concept beside the mechanism proof of concept. The negative proof shows the trigger state cannot be reached.

- Rate the finding from the reachable state only and downgrade it when the math forbids the trigger. Treat a finding with no reachable trigger as Low or informational, never as Medium or higher.

- Work through the pricing example when exits use net value. When exits are priced on net asset value, a trigger that needs gross value below pending fees cannot be reached by withdrawal alone.

## Example Triage Loop

    semgrep --config=auto --severity=ERROR --severity=WARNING --json

Then manually filter:

- drop tests, demos, fixtures, vendored code

- drop local-only or non-reachable paths

- keep only findings with a clear network or user-controlled route

## Report Structure

    ## Description
    [What the vulnerability is and why it matters]

    ## Vulnerable Code
    [File path, line range, and a small snippet]

    ## Proof of Concept
    [Minimal working request or script]

    ## Impact
    [What the attacker can achieve]

    ## Affected Version
    [Version, commit, or deployment target tested]

## Quality Gate

Before submitting:

- The code path is reachable from a real user or network boundary

- The input is genuinely user-controlled

- The sink is meaningful and exploitable

- The PoC works

- The issue is not already covered by an advisory, CVE, or open ticket

- The finding is checked against the known-issues list, prior audits, and disclosures, with live deployed state verified at a pinned block

- The current submission route is verified before drafting

- The program is confirmed active and submittable with a cash reward

- Impact is proved by execution against deployed bytecode when the finding depends on deployed behavior, with no impact figure or severity quoted before that execution passes

- The submission cost is recorded and the user confirmed it when a stake or fee applies, and the four submission gates are recorded with the source that confirmed each one

- The trigger state is proved reachable from the pricing and invariant math, with a mechanism proof of concept and a negative proof of concept

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Ask the user which programs they can afford to submit to before you pick the next audit target.

- Reuse a verified reachable-state proof across findings in the same codebase instead of proving each trigger from scratch.
