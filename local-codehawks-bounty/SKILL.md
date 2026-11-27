---
name: local-codehawks-bounty
description: Work CodeHawks (codehawks.cyfrin.io, Cyfrin) audit contests and First Flights — contest lifecycle, Impact x Likelihood severity, Lead Judge and appeals, prize splits, First Flights practice track. Use when auditing a CodeHawks contest, drafting a CodeHawks finding, or practising on First Flights. Free to enter; never submits in Sky's name.
---

# CodeHawks (Cyfrin) — Researcher Skill

CodeHawks (codehawks.cyfrin.io) is Cyfrin's competitive smart contract
auditing platform, launched 2023. Researchers review intentionally scoped
Solidity codebases, identify security issues, write reproducible findings,
and submit reports for judging. Contests can include monetary rewards,
rankings, or educational formats (First Flights). Cyfrin competes on
researcher quality over quantity: its own archive of 304 published audit
reports informs judging calibration. CodeHawks contests typically run 3-21
days with prize pools that vary by contest.

## Sky's rules (apply to every platform skill, no exceptions)

- FREE ONLY. CodeHawks contests and First Flights are free to enter — no
  stake, no entry fee. If any page asks for payment to participate, STOP
  and flag it.
- NEVER submit a finding in Sky's name. Prepare drafts and bundles; Sky
  submits himself, or gives an explicit order for a specific submission.
- CHECK KNOWN / FIXED BEFORE ANY SUBMISSION: read the contest README and
  known-issues list, prior CodeHawks reports for the same codebase, public
  disclosures, and the live deployed state at the pinned commit. Never
  submit a known or fixed finding.
- Report drafts for Sky are PLAIN TEXT: no `#` and no `*` characters anywhere
  in the draft.
- Do NOT create accounts on any platform unless Sky explicitly orders it.

## How a CodeHawks contest works

1. Contest page — protocol, scope repo + pinned commit, nSLOC, prize pool
   breakdown, timeline, rules, judging criteria, and known issues are
   published on codehawks.cyfrin.io.
2. Local audit — clone the contest repo, build with Foundry (or the
   contest's toolchain), run the test suite, map privileged roles and state
   transitions, then validate hypotheses with unit tests, fuzz tests, or
   invariant tests.
3. Submission — findings are submitted through the contest interface before
   the deadline. One finding per submission: title, severity, affected
   files/lines, description, impact, PoC, recommended mitigation.
4. Judging — a Lead Judge reviews submissions; community judging may also
   run. Judges publish a judgement per submission: Validated, Invalidated
   (with reason), or split/duplicated, and assign finding tags (e.g.
   M-01, H-01).
5. Appeals — a window to appeal judgements you disagree with, with new
   argument and evidence. Appeals are reviewed by the judges.
6. Rewards distribution — the pool is split per the published breakdown
   after judging and appeals close.

Contest phases shown on the platform: Live, Judging, Appeals, Appeals
Review, Rewards Distribution. Always check the CURRENT phase before doing
any work — a contest in Judging or later is closed to new submissions.

## Severity model — Impact x Likelihood

CodeHawks uses an Impact x Likelihood matrix with High / Medium / Low /
Informational tiers; a per-contest judge assigns the final severity.

- High — high likelihood AND high impact: directly exploitable, loss of
  funds or protocol-breaking behaviour with a realistic path.
- Medium — meaningful impact with conditions, or high impact at lower
  likelihood. State BOTH likelihood and impact explicitly in the finding
  (CodeHawks findings are expected to carry a Likelihood and an Impact
  section).
- Low — minor impact, best practice, or narrow conditions; paid only from
  the Low pool if the contest specifies one.
- Informational — no security impact; gas notes, style, documentation.

A finding that only says "this could happen if..." without likelihood and
impact reasoning gets invalidated as "Non-acceptable severity" or "Too
generic". Judges have invalidated findings for exactly that: vague
likelihood, assumed preconditions, or admin-trusted paths the README
excludes. Read the contest README's trusted-roles section first — issues
that depend on a trusted admin or keeper misbehaving are usually invalid.

## Prize pool breakdown

Each contest publishes its own split. Real examples from the platform:

- 50,000 USDC contest (1,910 nSLOC): High/Medium pool 47,500 USDC,
  Low pool 2,500 USDC.
- 20,000 USDC contest (662 nSLOC): High/Medium 16,000 USDC, Low 2,500
  USDC, Judges 1,500 USDC.

So roughly 80-95% of the pool goes to High/Medium findings, a small slice
to Low, and a slice to judges. Solo, unique High findings pay the most;
duplicates split the severity bucket. Prioritise unique findings and submit
early — the platform shows submissions publicly after judging, so
originality is visible.

Payouts are in USDC. Identity verification may be required before the first
payout — check the contest terms; Sky does verification himself if needed.

## First Flights — the practice track (FREE, no cash)

First Flights are educational auditing exercises on CodeHawks, designed for
beginners and for building a track record:

- Smaller codebases with KNOWN vulnerabilities planted in them.
- No monetary prizes — XP-based leaderboard progression (e.g. First Flight
  #10: One Shot, 100 EXP).
- Same submission process as real competitive audits, and judges review
  every finding and provide feedback.
- AI First Flights are always available on-demand, with instant AI-powered
  feedback on findings.
- Practice repos are public: https://github.com/Cyfrin/first-flights

Recommended path for a new auditor: do 3-5 First Flights, submit every
finding you can, learn from the judge feedback, THEN enter paid contests.
First Flights are the cheapest way to learn what judges accept and reject —
the feedback is the product, not the XP.

Local workflow for a First Flight:

    git clone https://github.com/Cyfrin/first-flights.git
    cd first-flights
    forge install
    forge build
    forge test -vvv

## Finding format (what judges want)

A useful CodeHawks finding normally includes:

- Title — specific, names the function and the flaw.
- Summary — one paragraph: what is wrong, where, with what consequence.
- Vulnerability Details — the code path, attacker/user preconditions, exact
  file:line references at the pinned commit, and why intended behaviour is
  violated. Include the vulnerable code snippet.
- Risk — Likelihood (Low/Medium/High, with reasoning) and Impact
  (Low/Medium/High, with reasoning). Both are required; a finding without
  them reads as a guess.
- Proof of Concept — a coded reproduction (Foundry test preferred) that
  fails on the vulnerable code, with the exact file to add it to and the
  command to run it, plus observed output. Manual steps with exact values
  are acceptable only if code cannot reproduce it.
- Tools Used — manual review, static analysis, fuzzing, etc.
- Recommendations — concrete fix, ideally with a code sketch.

Write findings so a judge who has never seen the code can reproduce the bug
in under 10 minutes. If your PoC needs more setup than that, simplify it.

## Judging, tags, and appeals

- The Lead Judge publishes a judgement per submission with a reason:
  Validated, Invalidated (e.g. "Non-acceptable severity", "Too generic",
  "Incorrect statement"), or duplicate-merged into a tagged finding
  (M-01, H-01...).
- Assigned finding tags group duplicates: if your finding is merged into
  someone else's M-01, you share that bucket.
- Appeals: during the Appeals phase, submit a reasoned appeal with NEW
  evidence — a coded PoC the judge missed, a concrete exploit path, a
  misread of the code. "I disagree" without new evidence is ignored.
- Community judging exists on some contests: other researchers vote on
  validity. A finding the community rates 100% Invalid is dead; do not
  waste an appeal on it.
- Judge feedback on First Flights is the training data: read every
  judgement on your submissions, including the invalidated ones, and
  change your next finding accordingly.

## Tooling note

- Cyfrin's Aderyn static analyzer (Rust-based, built for contest triage)
  was formally archived as read-only by Cyfrin on 17 November 2026 after a
  period listed as retired. Do NOT build a workflow that depends on Aderyn
  being maintained; use it only if already installed, and verify its output
  by hand.
- Standard local stack: Foundry (forge build / forge test), git, and manual
  review. Slither and other analyzers are triage aids, never proof — a
  finding needs a running PoC, not a scanner flag.

## Pre-submission checklist (run every time)

1. Is the contest in the Live phase with a future deadline? (If no, stop.)
2. Is the code in scope, at the pinned commit? (If no, stop.)
3. Is the finding on the README known-issues list or in a prior CodeHawks
   report? (If yes, stop — do not submit.)
4. Is it already fixed in the live deployed code? (If yes, stop.)
5. Does the finding state Likelihood AND Impact with reasoning? (If no,
   rewrite.)
6. Does a coded PoC reproduce it against the pinned commit? (If no, do not
   submit a High/Medium.)
7. Is the draft plain text, no `#`, no `*`, with file:line references?
8. Has Sky approved this specific submission? (If no, draft only.)

## Common traps

- Submitting a "too generic" finding: an invariant that breaks in a test
  but has no realistic mainnet impact is Invalidated, not Low.
- Claiming High for a trusted-admin or keeper-dependent path the README
  excludes. Read the README before writing.
- Auditing the wrong commit or the wrong branch of the contest repo.
- Confusing First Flights with paid contests: First Flights pay XP only.
  Never promise Sky cash from a First Flight.
- Depending on Aderyn: archived Nov 2026, unmaintained. Verify everything
  by hand.
- Copying another researcher's public submission after judging opens.
  Duplicates share the bucket and plagiarism risks a ban.

## Sources

- CodeHawks platform: https://codehawks.cyfrin.io
- First Flights: https://codehawks.cyfrin.io/first-flights
- First Flights repos: https://github.com/Cyfrin/first-flights
- Prize-split examples and judging behaviour verified against public
  CodeHawks contest pages (2025-02-gamma, 2024-08-fjord, 2024-02-one-shot),
  Nov 2026.
- Platform comparison (3-21 day contests, Impact x Likelihood model,
  Aderyn archived 17 Nov 2026, 304 Cyfrin reports) verified against
  smartcontractaudit.com and smartcontractshacking.com CodeHawks pages,
  Nov 2026.
