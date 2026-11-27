---
name: yeswehack
description: YesWeHack bug bounty platform — credits per submission, MangoPay KYC gate, CVSS form scoring, triager-grade report structure, Dojo practice, private-program invites, official Claude Kit skills. Use when hunting or reporting on any YesWeHack program, or checking whether a YesWeHack program passes Sky's FREE-only gate.
---

# YesWeHack — Bug Bounty Platform

YesWeHack is the largest European bug bounty platform, GDPR-compliant, at
yeswehack.com. Hunters find vulnerabilities on public Bug Bounty programs
and Featured VDPs, and high-quality reports earn invites to private
programs. The platform also runs Dojo, a free practice environment with
challenges, and publishes an official Claude Code plugin, the YesWeHack
Claude Kit, with three skills for report writing and triage.

## 1. How the platform works

- Programs are public or private. Public programs are visible to every
  registered hunter. Private programs are invite-only: invitations come
  from a record of complete, valid reports on the platform.
- Each program defines exactly what is allowed: domains, APIs, mobile apps,
  and the accepted vulnerability types. Read the scope and rules BEFORE
  testing. Going out of scope can get the account banned.
- Reports earn rewards and ranking points for the leaderboard. Quality
  reports earn private invites. Poor reports burn credits and reputation.
- Bounties are paid into an e-wallet on the platform, visible under the
  Transactions tab. Payout runs through the platform's payment provider.
- The platform's own program (yeswehack.com itself) is a useful reference
  for the house rules: bounty EUR 50 to EUR 15,000, 4 domains in scope, no
  denial-of-service testing, no interference with network equipment, no
  automated-tool abuse, report within 24 hours of discovery and exclusively
  through yeswehack.com, first reporter only, no disclosure including
  partial disclosure.

## 2. The credit system (the gate that matters most for Sky)

- Each Bug Bounty or Featured VDP program has a SUBMISSION COST in credits.
  If the credit balance does not cover the cost, the report cannot be
  submitted. The cost is per program and is shown on the program page.
- Minimum cost per report is 2 credits. Budget at least 2 credits per
  report, and check the balance before drafting, not after.
- Bonus credits can be earned depending on report status and severity, so
  a valid accepted report can replenish the balance. A rejected or
  out-of-scope report spends credits and returns nothing.
- Regularly check the credit balance when several reports are still in
  progress. A balance that covers one report may not cover the next.
- Credits make YesWeHack a pay-per-report platform in practice, even
  though registration is free. A program passes Sky's FREE gate only when
  Sky already holds enough credits to submit at zero additional cost. If
  credits must be bought, the program is NOT free and needs Sky's explicit
  cost approval first.
- The Dojo program is exempt from the credit and KYC gates and stays
  accessible to hunters who are not KYC-verified. Use Dojo to practice
  report structure at zero cost.

## 3. KYC gate (MangoPay)

- To submit a vulnerability report on a Bug Bounty or Featured VDP program,
  the hunter must be KYC-verified. Without a confirmed identity, complete
  the KYC process first. Once the profile is validated, submission unlocks.
- KYC runs through MangoPay, the platform's payment and identity provider.
  Fill in payment details and complete the identity check in the profile.
  Enable two-factor authentication: it is required for many programs and
  protects the account.
- The Dojo program is exempt and remains accessible without KYC.
- Sky-only unlock: the YesWeHack account, the MangoPay KYC, and the credit
  balance are all Sky's to create and hold. Bang never creates the account,
  never completes KYC for Sky, and never buys credits. If any of the three
  is missing, the YesWeHack route is blocked until Sky completes it.
  Do not draft a submission for a program Sky cannot submit to yet.

## 4. Severity and CVSS

- Severity is scored with CVSS in the submission form. The hunter assesses
  each CVSS metric and the form calculates the score.
- Severity levels used across programs: Critical, High, Medium, Low, and
  sometimes Informational (usually unpaid). The program page shows the
  bounty for each level in the program's own currency.
- The platform's own analysts assess reports on worst-case exploitation,
  and the reward follows that assessment. Write the impact for the worst
  case you can DEMONSTRATE, not the worst case you can imagine.
- Many programs pay nothing for Low severity. Check the program's reward
  table before investing time in a Low.
- CWE is form metadata, not body text. YesWeHack renders the CWE
  automatically from the form. Do not put a CWE ID in the report body.

## 5. How to submit (step by step)

1. Register a hunter account (Sky only), complete the profile, enable 2FA,
   complete MangoPay KYC, and confirm the credit balance covers the
   program's submission cost (minimum 2 credits).
2. Read the program description and rules in full, and the Platform Code of
   Conduct. Note the scope, the accepted vulnerability types, the reward
   table, and any program-specific rules.
3. Run the pre-submit known/fixed check (section 8). If the finding is
   known, a duplicate, or out of scope, stop. Do not spend credits on it.
4. On the program page, click "Submit report" to open the form.
5. Fill every field with the required bug details: title, description,
   vulnerability discovery, PoC, exploitation steps, impact, remediation,
   references, and the CVSS metrics.
6. Use the title auto-generation button if needed. The title format is:
   vulnerability class in location via parameter or header, under 100
   characters, no marketing words.
7. Attach images or videos to help triagers assess the report (optional
   but strongly recommended for anything visual or multi-step).
8. Check the box if the bug chains to another report.
9. Click "Submit", confirm the submission, and verify the report appears in
   the Reports tab with status "New". If it does not appear, the submission
   did not go through — do not assume it did.
10. Track updates through the bell icon notifications. Answer triager
    questions in the report thread.

## 6. Report structure (triager-grade, from the official Claude Kit)

The official YesWeHack Claude Kit enforces seven ordered body sections.
A draft missing any section is not ready. The kit's three skills are
installed in the global skill store as write, triage and gotchas — use
them when shaping a YesWeHack draft.

1. Description: 1 to 3 sentences stating what the bug is, factually and
   specifically. No CWE ID in the body. No multi-paragraph OWASP
   introduction or boilerplate definitions. Good example: Reflected XSS in
   /search via the q parameter. The parameter is echoed unescaped into the
   HTML response body.
2. Vulnerability Discovery: lab-notes narrative of the testing steps,
   observations and hypotheses. Keep dead ends and friction — they are
   credible signals of real investigation.
3. Proof of Concept: the exact request, payload, or steps that reproduce
   the bug. The triager must be able to copy it and see the same result.
4. Exploitation: numbered steps, one atomic action per step, written so the
   triager performs each step verbatim.
5. Impact: strictly limited to demonstrated damage. Never theoretical.
   Good example: arbitrary JS execution in the victim's browser in the
   target origin. I demonstrated reading document.cookie and exfiltrating
   it to a controlled endpoint in PoC step 4.
6. Remediation: concrete fix, short and specific.
7. References: links to documentation, standards, or prior work that
   support the report.

Title rule: under 100 characters, structured as vulnerability class in
location via parameter or header, no marketing words.

## 7. Report template (plain text for Sky's draft)

Sky's rule: drafts are plain text, with no # and no * characters anywhere.
Write the draft that way from the first line. Note the YesWeHack form takes
the body as structured text; keep the seven sections in order.

Title: Vulnerability class in location via parameter or header

Description: 1 to 3 sentences, factual and specific, no CWE ID, no
boilerplate.

Vulnerability Discovery: lab-notes narrative, steps taken, observations,
hypotheses, dead ends kept.

Proof of Concept: exact request or payload, expected and observed result.

Exploitation:
Step 1: one atomic action.
Step 2: one atomic action.
Continue until impact is demonstrated.

Impact: demonstrated damage only. What data, which users, what the attacker
did in the PoC. No theoretical claims.

Remediation: concrete fix.

References: supporting links.

CVSS: fill every metric in the form; record the vector and score here for
Sky to copy into the form.

## 8. Scope and asset rules

- Scope is per program and exact: domains, APIs, mobile apps, accepted
  vulnerability types. Anything outside it is out of scope, and testing it
  risks a ban.
- Do NOT test wallet or KYC features on the YesWeHack own program. Those
  are third-party services (MangoPay) and are explicitly out of scope.
- No denial-of-service attacks, no interference with network equipment or
  platform infrastructure, no manipulation or destruction of user data.
- Do not use automated tools abusively. On the YesWeHack own program the
  guidance is to limit request rate (around a few requests per second) and
  to refrain from automated scanning.
- Reports based on information whose source is not a failure of the
  organization or its employees or service providers are not accepted. Do
  not submit leaks or credentials bought, traded, or stolen from elsewhere.
- On the YesWeHack own program, reports about exposed secrets or
  credentials are eligible only when the source of the exposure is under
  YesWeHack's control, directly or indirectly.
- First reporter only. A duplicate of an already-reported vulnerability
  earns nothing and still costs credits.

## 9. Triage timeline and statuses

- After submission the report status is "New". Triage is done by the
  program's security analysts or program managers.
- There is no platform-wide triage SLA published for all programs. Response
  time varies by program and by severity. Track the report in the Reports
  tab and answer questions promptly; slow answers slow the payout.
- Rewards and ranking points are granted for complete and valid reports.
  Quality directly affects the bounty amount.
- Support: support@yeswehack.com for platform assistance.

## 10. Sky-specific facts

- YesWeHack route is a Sky-only unlock: account plus MangoPay KYC plus at
  least 2 credits per report. All three must exist before any YesWeHack
  submission is drafted. As of the last check, Sky has not confirmed any of
  the three as complete. Do not treat YesWeHack as submittable until Sky
  confirms them.
- Official Claude Kit (yeswehack/claude-kit, GPL-3.0) installed in the
  global skill store as write (208 lines), triage (113 lines) and gotchas
  (415 lines). The kit's skills are report-shaping tools only: they do not
  cover credits, KYC, program selection, or submission flow. This skill
  covers the platform mechanics the kit does not.
- The kit installs with bare names (write, triage, gotchas) because that is
  how the upstream repo names them. Their descriptions are YesWeHack
  specific, so they trigger on YesWeHack report tasks, not on general
  writing. If a bare name ever collides with a future skill, rename the
  local copy instead of overwriting.

## 11. Sky standing rules (apply to every YesWeHack report)

- FREE only. Registration is free, but a submission costs credits. A
  program is FREE for Sky only when his existing credit balance covers the
  submission cost at zero additional spend. Buying credits needs his
  explicit approval.
- Bang never submits a report in Sky's name. Prepare the draft and the PoC
  bundle; Sky submits from his own account.
- Before ANY submission, check first whether the finding is already known
  or fixed: known-issues list, prior audits, public disclosures, live
  deployed state. Never submit a known or fixed finding, and never spend
  credits on a duplicate.
- No destructive testing. Stay inside the program scope. Respect the
  request-rate guidance.
- Drafts are plain text with no # and no * characters.
- Bang never creates a YesWeHack account, never completes MangoPay KYC,
  and never buys credits. Those are Sky-only actions.

## 12. Common traps

- Drafting a full report before checking the credit balance, then finding
  the balance does not cover the submission cost. Check credits first.
- Spending credits on a duplicate or a known issue. The pre-submit check
  is worth more on YesWeHack than anywhere else, because a bad submission
  costs credits.
- Putting the CWE ID in the report body. The form renders it automatically
  from metadata; body CWE IDs mark the draft as amateur.
- Writing theoretical impact in the Impact section. The kit's triage skill
  returns DO NOT SUBMIT for that. Impact is demonstrated damage only.
- Testing wallet or KYC features on the YesWeHack own program. Explicitly
  out of scope, third-party services.
- Using marketing words in the title, or a title over 100 characters.
- Reporting a leak whose source is not the organization's failure. That is
  rejected and the credits are gone.

## Sources

- YesWeHack Help Center: Submit a vulnerability report
  (helpcenter.yeswehack.io, updated 2026-08-12) — submission steps, KYC
  gate, credit cost per program, bonus credits, Dojo exemption. Read live
  2026-11-23.
- YesWeHack Claude Kit: yeswehack/claude-kit (GitHub, GPL-3.0) — write,
  triage, gotchas skills; official announcement on yeswehack.com, "Write
  triager-grade Bug Bounty reports with our Claude Kit plugin".
- YesWeHack own program policy (huntdeck.io mirror of the program page) —
  scope, eligibility, 24-hour reporting rule, no-disclosure rule.
- Sky hunt memory: YesWeHack unlock status (account plus MangoPay KYC
  plus at least 2 credits per report, Sky-only).
