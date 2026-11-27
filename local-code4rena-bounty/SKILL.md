---
name: local-code4rena-bounty
description: Work with Code4rena (code4rena.com) as a warden — contest model, warden/judge roles, OWASP severity, share-based rewards, submission rules. STATUS: Code4rena announced in May 2026 it is closing; use this skill for its public report archive and Immunefi migration, not for new contests. Use when reading C4 reports, checking prior art, or verifying a C4 contest is really closed.
---

# Code4rena (C4) — Warden Skill

Code4rena (code4rena.com, docs at docs.code4rena.com) was the original
competitive audit platform: sponsors put up a prize pool, independent
researchers ("wardens") compete to find vulnerabilities in a fixed window,
judges evaluate and deduplicate findings and assign severity, and the pool is
split by severity and uniqueness. Founded 2021; acquired by Zellic (Aug
2024); 16,600+ registered wardens; 512 completed audits, 26,898 findings,
1,607 unique High-severity vulnerabilities in its public archive.

## STATUS WARNING — CODE4RENA IS CLOSING (May 2026)

Code4rena announced in May 2026 that it is CLOSING ITS DOORS. Active
competitions are being completed and bug bounty customers were moved to
Immunefi (Immunefi onboarded C4's bug bounty customers, porting scopes,
rules, and rewards). As of Sep 2026 the platform is best used as a large
PUBLIC ARCHIVE of contest reports, not as a place to enter new audits.

- Do NOT plan a new Code4rena submission, and do NOT tell Sky a C4 contest
  is live, without FIRST checking code4rena.com/audits live. If the audit
  list shows no new contests, the platform is closed and the answer is no.
- If a live contest somehow still exists (wind-down tail), treat it like any
  other contest below — but verify the payout route and the Immunefi
  migration notice first.
- The archive remains HIGH VALUE for free: every report is public, with
  findings, judge comments, and severity reasoning. Use it as a prior-art /
  known-issues source before submitting the same finding anywhere else.
- Bug bounty scopes that lived on C4 now live on Immunefi — check Immunefi
  for the current program, rules, and reward, never the stale C4 page.

## Sky's rules (apply to every platform skill, no exceptions)

- FREE ONLY. C4 contests were free to enter (no stake, no fee; 100% of
  sponsor funds went to wardens and judges after the Zellic acquisition).
  If any "C4" page asks for payment, it is not the real C4 — stop and flag.
- NEVER submit a finding in Sky's name. Prepare drafts and bundles; Sky
  submits himself, or gives an explicit order for a specific submission.
- CHECK KNOWN / FIXED BEFORE ANY SUBMISSION: read the contest known-issues
  list, prior C4 reports for the same codebase, public disclosures, and the
  live deployed state at the pinned commit. Never submit a known or fixed
  finding.
- Report drafts for Sky are PLAIN TEXT: no `#` and no `*` characters anywhere
  in the draft.
- Do NOT create accounts on any platform unless Sky explicitly orders it.

## How a C4 contest worked (archive reference)

1. Contest opens — sponsor, scope repo + pinned commit, prize pool, window
   (most contests ran 3-7 days, starting and ending at 20:00 UTC), rules,
   and known issues published at code4rena.com/audits.
2. Wardens audit — any registered warden could join; contests were open
   invitation for all skill levels. Code was made public at audit time in
   most cases (KYC-restricted and private competition options existed).
3. Submission — findings submitted through the contest UI before the
   deadline. Each finding needed an impact statement, severity rating, and
   PoC. Since the Zellic acquisition: mandatory PoCs and in-app / live
   judging.
4. Judging — judges deduplicated, assigned severity, and graded. Wardens
   could formally dispute a severity rating.
5. Mitigation review — selected wardens reviewed the sponsor's fixes.
6. Payout — in USDC/USDT, about 2 weeks after contest close. Identity
   verification was required before the first payout.

Prize pools typically ran $10K-$200K (some sources report up to $500K);
sponsors set the pool by scope, with C4 guidance. C4 charged no platform
fee for competitions.

## Roles

- Warden — the researcher. Finds bugs, writes findings, earns pool shares.
- Judge — evaluates findings, sets validity and severity, handles disputes.
  Judges were paid from the pool; a judge could also compete as a warden
  but forfeited their winnings.
- Sponsor — the protocol paying the pool; provides scope, code, docs, and
  answers questions during the contest.
- C4 team — ran intake, scoping, and the platform. Code and docs had to be
  delivered at least two business days before start; audits were locked in
  only after a deposit.

## Severity criteria (OWASP-based)

C4 assessed severity on an OWASP-based methodology, in three primary risk
categories plus Low/non-critical:

- High — significant financial loss or protocol degradation; directly
  exploitable or breaks core accounting. (Solo High in a $100K pool could
  pay $5K-$20K.)
- Medium — limited financial impact or conditional exploit; needs specific
  conditions or a privileged actor. (Solo Medium in a $100K pool: $1K-$5K.)
- Low / Non-critical — minor issues, best practice, gas optimisations.
  Paid via a QA report curve (top three QA reports), typically $50-$500
  total for a QA report.

High-level considerations judges weighed: malicious input handling,
escalation of privileges, arithmetic, and gas use. Severity was what the
JUDGE assigned after deduplication and dispute — argue from impact and
likelihood, with preconditions stated honestly.

## Reward formula (share-based)

Findings earned shares, and each warden got a pro-rata piece of the severity
bucket based on their shares:

- High Risk shares = 10 * (0.9 ^ discovery_count) / discovery_count
- Medium Risk shares = 3 * (0.9 ^ discovery_count) / discovery_count
- Low Risk shares = 1 * (0.9 ^ discovery_count) / discovery_count

where discovery_count is the number of wardens who found the same issue.
Duplicates penalise every finder: a solo High is worth far more than a
5-way duplicated High. Bonuses existed for selected reports and for the
wardens with the most unique and most valid findings. Consequence for the
hunter: uniqueness pays; prioritise unusual code paths and cross-contract
interactions, and submit unique findings fast.

## Submission rules (archive reference)

- One finding per issue. Do not bundle several bugs into one report — judges
  split them and you lose clarity.
- Each finding: title, severity claim, affected files/lines, description,
  impact, coded PoC, recommended mitigation.
- PoC mandatory (post-acquisition rule). A Foundry test that fails on the
  vulnerable code and passes on fixed code is the gold standard. Never
  submit raw LLM output — judges suspended accounts for slop.
- Deadline is hard: contests ended at 20:00 UTC on the closing day.
  Findings submitted after the deadline were not judged.
- Wardens were expected to read the submission guidelines and judging
  criteria first — they govern what counts as High/Medium.

## Using the C4 archive (the live value today)

- Reports index: https://code4rena.com/reports and https://code4rena.com/audits
- Per-contest repos: github.com/code-423n4 (one repo per contest, with the
  exact scoped code at the contest commit).
- Use the archive to: (a) check whether a finding on a live target was
  already found in a C4 contest for the same codebase; (b) learn judge
  severity reasoning from judge comments; (c) study winning finding
  write-ups as report templates; (d) find the Immunefi program that
  inherited a C4 bounty scope.
- When a live target's code matches a C4 contest repo, diff the contest
  commit against the live code BEFORE auditing — unchanged code means the
  C4 findings are still the known-issues list.

## Pre-submission checklist (only if a live C4 contest is verified)

1. Is the contest actually live on code4rena.com/audits right now, with a
   future deadline? (If no, STOP — platform is closed.)
2. Is the payout route confirmed (C4 direct, or migrated to Immunefi)?
   (If unclear, stop and verify.)
3. Is the code in scope, at the pinned commit? (If no, stop.)
4. Is the finding on the known-issues list or in a prior C4 report?
   (If yes, stop — do not submit.)
5. Is it already fixed in the live deployed code? (If yes, stop.)
6. Does a coded PoC reproduce it against the pinned commit? (If no, do not
   submit a High/Medium.)
7. Is the draft plain text, no `#`, no `*`, with file:line references?
8. Has Sky approved this specific submission? (If no, draft only.)

## Common traps

- Assuming C4 is still open. It announced closure in May 2026 — always
  verify liveness first. A stale blog post or aggregator listing is not
  proof of a live contest.
- Following a C4 bug bounty link: those customers moved to Immunefi. The
  C4 page is stale; the Immunefi page is the current scope and reward.
- Submitting a finding already in the C4 archive for the same codebase —
  judges and sponsors both check the archive.
- Treating a QA/gas report as income: QA paid on a curve to the top three
  only, and small.
- Submitting raw AI-generated findings: judges suspended accounts for slop.
  Every finding must be hand-verified with a running PoC.

## Sources

- Code4rena homepage and audits list: https://code4rena.com and
  https://code4rena.com/audits
- Code4rena reports archive: https://code4rena.com/reports
- Code4rena docs: https://docs.code4rena.com
- Closure (May 2026, move to Immunefi) verified against web3security.ai
  Code4rena profile (researched Sep 2026) and The Block reporting, Nov 2026.
- Reward formula, pool ranges, and judging rules verified against C4 docs
  and the 2026 audit-contest income guide (slop.sus.cat), Nov 2026.
