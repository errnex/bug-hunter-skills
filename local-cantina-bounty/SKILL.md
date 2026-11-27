---
name: local-cantina-bounty
description: Work Cantina (cantina.xyz) audit competitions as a security researcher — competition lifecycle, Cantina Code submissions, severity criteria, points scoring and duplicate scaling, PoC rules, judging and escalation, Fellowship tiers, rewards and payout. Use when auditing a Cantina competition, drafting a Cantina finding, or checking whether a Cantina contest is worth entering. Flags Sky's FREE-only rule and never submits in his name.
---

# Cantina Audit Competitions — Researcher Skill

Cantina (cantina.xyz, docs at docs.cantina.security) is the public competition
arm of Spearbit — the two brands formally merged in May 2025 into one security
stack. Cantina runs time-bound competitive audits, managed bug bounties, and
private reviews. As of the 2026 review cycle it reports $46.7M paid out across
200+ protocols, including Uniswap v4 ($2.35M competition) and the Ethereum
Foundation Pectra review ($2M).

## Sky's rules (apply to every platform skill, no exceptions)

- FREE ONLY. Entering a Cantina competition costs nothing — no stake, no entry
  fee. If a competition ever asks for payment to participate, STOP and flag it.
- NEVER submit a finding in Sky's name. Prepare drafts and bundles; Sky
  submits himself, or gives an explicit order for a specific submission.
- CHECK KNOWN / FIXED BEFORE ANY SUBMISSION: read the contest known-issues
  list, prior audit reports for the same codebase, public disclosures, and the
  live deployed state at the pinned commit. Never submit a known or fixed
  finding.
- Report drafts for Sky are PLAIN TEXT: no `#` and no `*` characters anywhere
  in the draft (platform web forms and plain-text email choke on markdown).
- Do NOT create accounts on any platform unless Sky explicitly orders it.

## How a Cantina competition works

1. Scoping and information gathering — scope, target repos, commit hashes,
   prize distribution are fixed in a Competition Submission Template.
2. Statement of Work (SOW) — agreement with scope, deliverables, timelines.
3. Kickoff — project introduction and competition rule walkthrough.
4. Competition period — researchers audit the codebase and submit findings.
   Most competitive audits run 1-4 weeks.
5. Judging — findings are severity-classified and deduplicated; researchers
   may escalate disputes.
6. Fix period — sponsor fixes findings with Cantina team support.
7. Findings call, final report delivery, closeout call.
8. Judging and report delivery add 1-2 weeks after the review period ends.

Two audit formats exist. Competitive Audit: many independent researchers audit
simultaneously, findings ranked by severity and rewarded from a shared prize
pool — maximum coverage. Private Audit: a dedicated matched team works
exclusively on the code — for sensitive codebases or tight timelines.

## Where to find competitions

- Live list: https://cantina.xyz/competitions
- Each competition page shows scope repos + pinned commit, nLoC, prize pool
  split, timeline, rules, known issues, and the leaderboard.
- Always work from the PINNED COMMIT in the competition scope. Code that
  changed after the pin is out of scope; findings against unpinned code are
  invalid.

## Submission process (Cantina Code)

- Findings are submitted through Cantina Code (the platform UI) in a
  standardized format: title, severity, affected files/lines, description,
  impact, PoC, recommended mitigation.
- PoC rule (default): ALL High and Medium severity submissions must be
  accompanied by a coded Proof of Concept BEFORE the competition ends. This
  applies to researchers with a reputation score below 80. Researchers at
  reputation 80+ are exempt from the mandatory-PoC rule — but a coded PoC
  still wins judging disputes, so write one anyway.
- A PoC must be a RUNNING artefact, not a narrative: a Foundry test, a
  script, or a step-by-step reproduction with exact commands and observed
  output. A model or simulation PoC never establishes impact; only execution
  against the real code at the pinned commit does.
- You can withdraw a submission at any time via the "withdraw finding" option
  in the finding menu. Withdraw a finding the moment you realise it is a
  duplicate, known, or wrong — a withdrawn finding costs nothing; a judged
  invalid one costs reputation.
- Submission timing does not affect scoring — findings are scored equally
  whenever submitted during the competition. Submit early anyway: early,
  well-written reports let the team give feedback and start fixing.

## Severity criteria

Cantina judges severity on impact and likelihood of exploitation, following
its published Finding Severity Criteria:

| Severity | Meaning | Example |
| --- | --- | --- |
| Critical | Immediate risk of fund loss | Unchecked reentrancy draining funds |
| High | Significant impact, with conditions | Access control bypass |
| Medium | Moderate impact | Incorrect accounting logic |
| Low | Minor / best-practice violation | Unlocked pragma |
| Informational | No security impact | Gas optimisation |

Severity is what the JUDGE assigns, not what you claim. Argue severity from
impact + likelihood + preconditions, never from vibes. A finding that needs
an admin to act maliciously, or that only fires under conditions the protocol
controls, is usually downgraded. Read the contest's own severity page and
known-issues list before labelling anything High.

## Scoring and prize distribution

Cantina uses a transparent points-based model. Only High and Medium findings
earn from the Primary Prize Pool; Low / Informational / Gas findings may earn
from a separate reserved pool if the competition specifies one, awarded on
QUALITY not quantity.

- High severity finding: 10 points
- Medium severity finding: 3 points
- Low severity findings: separate pool only, if specified

Duplicate scaling — when multiple researchers submit the same bug, each
person's score is scaled down:

    Points = Full Points / (1 + 0.8 * (n - 1))

where n is the number of duplicate submissions.

- 2 duplicates: each receives 4.5 points for a High
- 3 duplicates: each receives 2.7 points for a High

All eligible participants who score points share in the public prize pot,
pro-rata by points. Consequence for the hunter: uniqueness pays. A solo High
in a large pool is worth far more than a 5-way duplicated High. Prioritise
unusual code paths, novel invariants, and cross-contract interactions that
other researchers are less likely to spot, and submit unique findings fast.

## Judging and escalation

- Judges evaluate each finding on impact and likelihood, deduplicate, and
  assign final severity. They may seek sponsor input.
- Escalation: after initial judging, the ORIGINAL SUBMITTER ONLY may appeal a
  decision, with additional context or justification for why the severity
  should be reconsidered.
- PENALTY: an invalid escalation costs $100, deducted from any future
  competition earnings. Never escalate on hope — escalate only with new
  evidence (a working PoC the judge missed, a concrete exploit path, a
  misread of the code).
- Cantina's decision after escalation is FINAL.

## Fellowship tiers (researcher reputation)

Three tiers gate access and bonus payouts:

- Apprentice — entry tier; $250 top-50% bonus.
- Resident — Spearbit ASR/SR designation, or $10K-$100K lifetime earnings;
  $1K-$2.5K top-25% bonus.
- Fellow — Spearbit LSR designation, or $100K+ lifetime earnings;
  $2.5K-$10K top-5% bonus.

Reputation below 80 triggers the mandatory-PoC rule above. Protect
reputation: submit fewer, stronger findings rather than many weak ones.

## Rewards and payout

- Prize pools are set per competition by the sponsor (typical range
  $50K-$200K; flagship competitions run into seven figures).
- Payouts are in USDC or the sponsor's token, per the competition terms.
- Platform/judging take exists: e.g. the Aave v3.1 competition budget was
  $195K total = $150K prize pool + $45K platform/judging (~23%).
- Protocols that complete a full review + competition + bounty may be
  eligible for up to $300K complimentary post-launch coverage for the first
  30 days (Nexus Mutual partnership).

## Pre-submission checklist (run every time)

1. Is the code in scope, at the pinned commit? (If no, stop.)
2. Is the finding on the known-issues list or in a prior audit report?
   (If yes, stop — do not submit.)
3. Is it already fixed in the live deployed code? (If yes, stop.)
4. Is there a coded PoC that runs against the pinned commit? (High/Medium:
   mandatory if reputation < 80.)
5. Is the severity argued from impact + likelihood, with preconditions
   stated honestly?
6. Is the draft plain text, no `#`, no `*`, with file:line references?
7. Has Sky approved this specific submission? (If no, draft only.)

## Common traps

- Auditing the wrong commit. Always `git checkout <pinned commit>` first.
- Submitting a finding the sponsor already lists as known — instant invalid,
  reputation hit.
- Claiming High for an admin-trusted or keeper-dependent path the contest
  explicitly excludes. Read the README's "out of scope / trusted roles"
  section first.
- Escalating without new evidence — the $100 penalty is real and cumulative.
- Copying another researcher's finding. Duplicates split points AND the
  later submission is worth less; plagiarism risks a ban.
- Treating Low/Info volume as income. That pool is quality-judged and small;
  one excellent Medium beats fifty gas notes.

## Sources

- Cantina docs, Competitions: https://docs.cantina.security/security-teams/services/competitions
- Cantina docs, How Audits Work: https://docs.cantina.security/security-teams/services/how-audits-work
- Cantina docs, Judging Processes: https://docs.cantina.security/standards/judging
- Cantina competitions list: https://cantina.xyz/competitions
- Platform figures ($46.7M paid, Fellowship tiers, fee example) verified
  against the 2026 Cantina review, m3dython.com, and Cantina docs, Nov 2026.
