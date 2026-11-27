---
name: intigriti
description: Intigriti bug bounty platform — program tiers, Exceptional to Low severity, CVSS scoring, validation-time SLAs, Onfido identity verification, @intigriti.me testing rules, Lightspark and 1Password program facts. Use when hunting or reporting on any Intigriti program, or checking whether an Intigriti program passes Sky's FREE-only gate.
---

# Intigriti — Bug Bounty and Agile Pentesting Platform

Intigriti is a European bug bounty platform (Belgium, EU-registered) at
app.intigriti.com. Programs list assets in Tier 1 to Tier 4, each tier with
its own reward table in EUR. Researchers register free, pick a public
program, test inside its scope, and submit through the platform. Private
programs are invite-only, driven by reputation and leaderboard standing.

## 1. How the platform works

- A program page has: scope (domains, URLs, wildcards, mobile apps, other
  asset types), tier tables with min and max rewards, rules, out-of-scope
  list, validation-time promises, and safe-harbor terms.
- Asset tiers: Tier 1 is the most critical asset (highest rewards), Tier 4
  the least. Example reward ranges from live programs: Intigriti's own
  program Tier 2 EUR 50 to EUR 13,337, Tier 3 up to EUR 5,000, Tier 4 up to
  EUR 1,500; TrueLayer Tier 1 EUR 175 to EUR 6,000; Trouw Tier 2 EUR 50 to
  EUR 2,200. Every program sets its own table. Always read the program's
  own table.
- The researcher sets the severity on submission (Exceptional, Critical,
  High, Medium, Low) plus a CVSS score. Triage can change both.
- Safe harbour applies on most programs: good-faith testing inside scope
  is protected. Testing outside scope, using automated scanners where
  forbidden, or disclosing without consent voids it.
- Do not discuss or disclose vulnerability information without prior written
  consent from the company, including PoCs on YouTube and Vimeo. This is
  stated on program after program and is enforced.

## 2. Severity and CVSS

- Severity levels: Exceptional, Critical, High, Medium, Low.
- Exceptional is for impacts beyond ordinary Critical (very large scale or
  chained impact). Use it rarely and only when the program's table has an
  Exceptional row with a higher reward. Do not inflate Critical to
  Exceptional to chase a bigger number; triage downgrades inflation and it
  hurts credibility.
- CVSS 3.1 is the standard; CVSS 4.0 is also accepted on newer programs.
  Fill every CVSS metric honestly. A wrong vector that inflates the score
  is a downgrade signal for triagers.
- Severity is set by impact, not by bug class. An XSS that only self-affects
  is Low. The same XSS that steals another user's session is High or
  Critical. State the victim and the impact in the first sentence.

## 3. Rewards and validation times

- Rewards are per program and per tier, in EUR unless the program states
  otherwise. There is no platform-wide table.
- Validation times are promised PER PROGRAM and start once Intigriti has
  verified the submission. Examples from live program pages:
  - Intigriti's own program: Exceptional 2 working days, Critical 2,
    High 5, Medium 15, Low 15.
  - TrueLayer: Exceptional 5 working days, Critical 5, High 10,
    Medium 20, Low 30. Working hours Monday to Friday 9am to 5pm GMT/BST.
- Submissions validated outside the promised time may be awarded a EUR 25
  bonus. This is at the company's discretion, not automatic.
- "Check our fix" bonus: up to EUR 50 to verify a resolved issue when the
  company asks you to. Also discretionary.
- Payout is created automatically when a company accepts a submission with
  a bounty. On acceptance the researcher is entitled to 100 percent of the
  bounty immediately.
- Payout methods: bank transfer, PayPal, Payoneer (UPI for India only on
  some programs). Fiat only, no crypto rail.
- Payout timing: 1 to 3 working days inside Belgium, 3 to 5 working days
  international. Payout states: Error, Pending, Processing, Paid. An Error
  state usually means the payment method is invalid or the researcher is
  not ID-checked yet.

## 4. Identity verification (Onfido) — read this carefully

- Intigriti is EU-registered and must verify the identity of payment
  recipients for KYC, tax and bookkeeping. Verification runs through
  Onfido, a third-party identity provider. Information given to Onfido is
  processed and kept confidential.
- How it works (Intigriti Help Center, ID Verification Process): complete
  the profile with all personal details, select "Request ID check" in
  general information, choose the document and the country that issued it,
  photograph the document (front and back where required). Rejected photos
  can be retried immediately. Maximum THREE attempts per user. Acceptance
  updates the profile to checked and unlocks additional programs.
- Real name and address are required for verification, although the public
  username can be a pseudonym.
- ID verification is required to RECEIVE any payout. That part is settled
  and platform-wide.
- The open question is whether ID verification is required BEFORE
  SUBMISSION. History: it was payout-only for years. In October 2026
  Intigriti announced a platform shift: ID verification moved earlier to a
  submission requirement for public programs that offer monetary rewards,
  applied per program (opt-in). The shift targets new accounts, low
  confidence submissions and AI-assisted noise.
- DECIDED READING for Sky's programs (2026-11-22, rounds 7, 39, 40):
  ID-check-before-submit is a per-program opt-in. Lightspark and 1Password
  show NO opt-in flag in their program metadata or submit guidance, so for
  those two programs IDV is PAYOUT-ONLY. That decision rests on
  program-level evidence, not on the general platform blog.
- Mandatory check before every Intigriti submission: re-read the target
  program's own page and submit flow for an ID-check flag. If the program
  has since opted in, Sky must complete Onfido himself before submitting.
  Never assume the payout-only reading still holds for a program you have
  not re-checked. Bang never completes IDV for Sky; only Sky does it.

## 5. How to submit (step by step)

1. Register at login.intigriti.com (Sky only — Bang never creates
   accounts). The username is permanent, so choose carefully. Activate by
   email, enable 2FA, complete the profile and payment details.
2. Open the program page and read it end to end: scope, tiers, rules,
   out-of-scope list, validation times, testing instructions.
3. Run the pre-submit known/fixed check (section 9). If the finding is
   known, a duplicate, or out of scope, stop.
4. Test ONLY inside scope, using the program's testing instructions:
   - Many programs require testing mail to use ONLY the @intigriti.me
     address. Violating this can mean no bounty is awarded.
   - Send the X-Intigriti-Username: {username} request header where the
     program asks for it.
   - Respect the rate limit (commonly 5 to 10 requests per second).
   - Do not use automated scanners where the program forbids them. Several
     programs reject scanner-found submissions outright.
   - Use sandbox or test environments where the program provides them.
5. In the platform, open the program and click submit. Fill: title, severity,
   CVSS vector and score, description, steps to reproduce, impact, remediation,
   attachments (screenshots, and a video for complex bugs — triagers prefer
   video, Loom is commonly accepted).
6. Submit once. Answer follow-up questions in the same submission thread.
   Do not open a second submission for the same bug.

## 6. PoC requirement

- Every report needs reproduction steps a triager can follow verbatim, and
  evidence of the impact (screenshot, request/response, or video).
- PoC video is valued much more than a screenshot alone for complex bugs.
  Record the full chain from clean state to impact, with no cuts over the
  decisive step.
- State the exact environment: attacker account type and email (use the
  @intigriti.me address), victim account, endpoint, method, parameter.
- Theoretical impact without demonstration is downgraded or closed. Show
  the read, the write, the takeover, or the data, not the possibility of it.

## 7. Scope and asset rules

- Tier tables ARE the scope. An asset at Tier 4 pays Tier 4 money even for
  a Critical bug. Check the tier of your target before you invest hours.
- Out-of-scope lists are long and specific: other group brands sharing a
  codebase, third-party contact forms, Zimpler-style payment adjacencies,
  registration and login flows on some programs, and the standard list
  (API key disclosure without proven business impact, WordPress username
  disclosure, missing rate limits without impact, broken link hijacking
  without takeover). Read the program's own list; the standard list is not
  identical everywhere.
- Already known to the company from its own testing means duplicate, no
  reward. Check disclosed reports and the program's own history first.
- Recently disclosed zero-day vulnerabilities in commercial products with
  no patch or a patch less than 2 weeks old: give the vendor 2 weeks before
  reporting, and expect no bounty in that window on several programs.

## 8. Report template (plain text for Sky's draft)

Sky's rule: drafts are plain text, with no # and no * characters anywhere.
Write the draft that way from the first line. Intigriti's own title format
is: Bug Class: One-line impact.

Title: Bug Class: Exact impact in endpoint or feature

Description: Impact-first paragraph. What an attacker can do, to whom,
with what privileges. Endpoint, method, parameter, data exposed.

Steps to Reproduce:
Environment: attacker account (standard account, intigriti.me email),
victim account, browser or tool versions.
Step 1: one atomic action.
Step 2: one atomic action.
Continue until impact is visible. One action per step, no combined steps.

Impact: specific and quantified. What data, how many users, what the
attacker can do next. Only demonstrated impact.

CVSS 3.1 Score: number and severity, with the full vector.

Remediation: one to three sentence concrete fix, with code if it helps.

Attachments: screenshot or video link showing the impact.

## 9. Sky-specific program facts (verified in prior rounds)

- Lightspark (Intigriti): Tier 1 EUR 150 to EUR 5,000, $0 cost, no
  reputation minimum. Gates pass. IDV is payout-only per program-level
  evidence (no opt-in flag). L-1 lead and KI-2 lead open from round 7;
  rig ready, blocked on Sky's browser signup (no work email) and on a
  webhook token that expired 2026-11-29 08:30 UTC. Delete the test repo
  errnex/errnex.github.io after the test. Do not re-audit without new facts.
- 1Password (Intigriti): audited round 7, 2026-11-20, and again round 40
  (delta check, 0 new facts). webAuthn export lead is UNVERIFIED and needs
  a live-browser test only Sky can do. IDV decided payout-only by the same
  program-level method (no identity-check flag in the program record).
- General gate: a program is FREE only when submission cost, reputation
  minimum, KYC requirement and PoC requirement are all confirmed from the
  program page itself, including whether ID-check-before-submit is opted
  in for that program.

## 10. Sky standing rules (apply to every Intigriti report)

- FREE only. Any submission fee or stake is a hard stop unless Sky
  explicitly approves the cost.
- Bang never submits a report in Sky's name. Prepare the draft and the PoC
  bundle; Sky submits.
- Before ANY submission, check first whether the finding is already known
  or fixed: known-issues list, prior audits, public disclosures, live
  deployed state. Never submit a known or fixed finding.
- No destructive testing. No automated scanners where the program forbids
  them. No disclosure without company consent.
- Drafts are plain text with no # and no * characters.
- Only Sky completes Onfido ID verification, and only when a payout or a
  program opt-in requires it.

## 11. Common traps

- Testing with a personal email instead of the @intigriti.me address.
  Several programs state this alone forfeits the bounty.
- Forgetting the X-Intigriti-Username header where required, so requests
  look like unattributed traffic.
- Running a scanner on a program that bans scanners, then submitting the
  scanner's output.
- Inflating severity to Exceptional without an Exceptional row in the
  program's tier table.
- Submitting on a program that has since opted into ID-check-before-submit
  without re-checking the submit flow.
- Reporting a bug the company already knew from its own testing. That is a
  duplicate, not a finding.
- Disclosing on YouTube or Vimeo before the fix. Consent first, always.

## Sources

- Intigriti Help Center: ID Verification Process (kb.intigriti.com,
  updated 2025-05-05) and Payout lifecycle (updated 2024-04-25). Read live
  2026-11-23.
- Intigriti blog: What ID verification changes mean for public bug bounty
  programs at Intigriti (October 2026) — platform shift to submission-time
  IDV, per-program opt-in.
- Live program pages (app.intigriti.com): intigriti own program, TrueLayer,
  Trouw, KU Leuven — tier tables, validation times, testing headers.
- Sky hunt memory: Lightspark IDV decision 2026-11-22 (rounds 7, 39),
  1Password IDV decision round 40.
