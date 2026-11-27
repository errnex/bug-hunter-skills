---
name: local-sherlock-bounty
description: Work Sherlock (sherlock.xyz) audit contests as a Watson researcher — contest lifecycle, Watson bonding/staking, severity rubric, judging and escalation, rewards and exploit coverage. Use when auditing a Sherlock contest, drafting a Sherlock finding, or checking whether Sherlock is worth entering. Marks Sherlock NOT-FREE for Sky (250 USDC stake gate) and never submits in his name.
---

# Sherlock Audit Contests — Researcher Skill

Sherlock (sherlock.xyz, audits at audits.sherlock.xyz) runs timed competitive
audit contests with bonded researchers ("Watsons"), plus private collaborative
audits, an AI audit engine, and post-launch bug bounties. Its signature is
the bond: researchers stake USDC against their performance, and the platform
sells exploit coverage — up to $2M payout to a protocol if a Sherlock audit
misses a vulnerability that is later exploited. Homepage claims: 1,500+
audits and security reviews, 11,000+ registered researchers, 2,000+ criticals
found, $250B+ in TVL secured. Contest reports are public at
github.com/sherlock-audit (460+ contest repositories as of Sep 2026).

## GATE WARNING — SHERLOCK IS NOT FREE FOR SKY

READ THIS FIRST. Sherlock requires Watsons to STAKE USDC to participate in
contests — the standard stake is 250 USDC. Sky's liquid balance is about
$0.22-$5.86 and the 250 USDC stake is UNAFFORDABLE. This skill therefore
treats Sherlock contests as GATE-BLOCKED for Sky until he explicitly says he
has funded a stake.

- Do NOT present a Sherlock contest as an actionable opportunity for Sky
  unless the stake requirement is stated up front in the same message.
- Do NOT attempt to stake, deposit, or bond anything on Sky's behalf.
- The skill is still useful for: reading Sherlock's public contest reports
  (free, no stake needed) as a known-issues / prior-art source, learning
  judging standards, and preparing drafts IF Sky ever funds a stake.
- Sherlock's separate BUG BOUNTY track (live code, post-contest) may not
  require a stake — verify per-program before assuming; if it needs a stake
  or fee, it is blocked the same way.

## Sky's rules (apply to every platform skill, no exceptions)

- FREE ONLY. The 250 USDC Watson stake fails the free gate. Never spend
  Sky's money to enter.
- NEVER submit a finding in Sky's name. Prepare drafts and bundles; Sky
  submits himself, or gives an explicit order for a specific submission.
- CHECK KNOWN / FIXED BEFORE ANY SUBMISSION: read the contest known-issues
  list, prior Sherlock reports for the same codebase, public disclosures,
  and the live deployed state at the pinned commit. Never submit a known or
  fixed finding.
- Report drafts for Sky are PLAIN TEXT: no `#` and no `*` characters anywhere
  in the draft.
- Do NOT create accounts on any platform unless Sky explicitly orders it.

## How a Sherlock contest works

1. Contest announcement — protocol, scope repo + pinned commit, prize pool,
   window (typically 7-30 days; extended contests such as Aave V4 ran 6
   weeks), rules, and known issues are published on audits.sherlock.xyz.
2. Watsons audit — 200+ vetted Watson researchers review the code in
   parallel. A Lead Watson (senior researcher) anchors the contest.
3. Submission — findings are submitted as GitHub issues in the contest's
   dedicated judging repository (e.g. `sherlock-audit/2024-05-gamma-staking-
   judging`). Each issue is one finding: title, severity, summary,
   vulnerability detail, PoC, recommended mitigation.
4. Judging — a judging panel (lead + judges) curates the issues, decides
   validity and severity, and deduplicates. The sponsor may comment on
   validity.
5. Escalation — submitters may escalate a judging decision with new argument
   and evidence; judges rule and the decision stands.
6. Final report — Sherlock publishes a full audit report (PDF + markdown)
   with every valid finding, severity, and fix status.
7. Payout — a few weeks after the contest ends; prize split across valid
   unique findings, weighted by severity and rarity.
8. Coverage — protocols that pass may buy exploit coverage; if a missed bug
   is later exploited, coverage pays out (Euler Finance 2023: ~$4.5M
   honored on a $197M incident).

Typical prize pools: $50,000-$300,000 per contest. Payouts in USDC.

## The Watson bonding model

- Watsons stake USDC when they enter. The stake is skin-in-the-game:
  valid findings earn from the pool; poor, duplicate, or spam submissions
  lose staking rewards and can forfeit stake.
- Performance is tracked per Watson; strong history earns Lead Watson roles
  and private-audit invitations.
- Because the stake is real money, Sherlock judging is STRICTER than
  open-entry platforms. Low-effort, speculative, or duplicate findings are
  punished, not just ignored. Submit fewer, stronger findings.
- Never advise Sky to stake borrowed or shared funds. The stake is at risk.

## Severity rubric (from published Sherlock reports)

Sherlock reports assign each issue a severity. The working definitions used
in final reports:

- High — directly exploitable security vulnerabilities that need to be
  fixed. Direct loss of funds or protocol-breaking behaviour with a
  realistic path.
- Medium — security vulnerabilities that may not be directly exploitable or
  may require certain conditions in order to be exploited. All major issues
  should be addressed.
- Low / Informational — minor issues, best-practice deviations, gas notes;
  paid only if the contest specifies a low pool, and judged on quality.

Argue severity from exploitability + impact, with preconditions stated
honestly. A finding that requires an admin to misbehave, a keeper to be
compromised, or a condition the protocol controls is usually downgraded or
invalidated. Read the contest README's trusted-roles and out-of-scope
sections before writing anything.

## Submission format (judging repo issue)

One GitHub issue per finding in the contest judging repo:

- Title: short, specific, names the function and the flaw.
- Summary: one paragraph — what is wrong, in which function, with what
  consequence.
- Vulnerability Detail: the code path, attacker/user preconditions, exact
  file:line references at the pinned commit, and why the intended behaviour
  is violated.
- PoC: a coded reproduction (Foundry test preferred) that FAILS on the
  vulnerable code and PASSES on fixed code, or exact manual steps with
  observed output. A narrative without a runnable PoC is weak on Sherlock.
- Recommended Mitigation: concrete fix, ideally with a code sketch.
- Impact: who loses what, under which conditions, with numbers when
  possible (amounts, percentages, token counts).

Sherlock judges read many submissions; the clearest, most reproducible
finding wins the severity argument. Put the PoC first in your own workflow:
if you cannot reproduce it, do not submit it.

## Judging, escalation, duplicates

- Judges deduplicate aggressively. A finding submitted by several Watsons
  splits the reward; the earliest, clearest submission anchors the issue.
- Escalation is a formal comment on the judging issue with NEW reasoning or
  evidence. "I disagree" without new evidence is ignored and damages
  reputation.
- Sponsor comments matter: if the protocol team demonstrates the behaviour
  is intended or the precondition is impossible in production, the finding
  dies. Check the sponsor's replies on similar issues before escalating.
- Sherlock's judging history is public — read past judging repos for the
  same protocol family before submitting. A pattern rejected twice will be
  rejected a third time.

## Rewards and payout

- Pool split is algorithmic: based on number, severity, and rarity
  (uniqueness) of valid submissions. Solo High findings pay the most;
  heavily duplicated findings pay little.
- Medium and High are the paid tiers in most contests; Low findings are
  usually unpaid unless a low pool is specified.
- Payouts land a few weeks after contest close, in USDC, to the Watson's
  registered payout address. Keep Sky's payout address on file only if he
  provides it: his Solana address is
  JCVmVfzSEPNEJvchLpPxx99mvVEmbxoePCcDCFx7Bo7X (do not use it for
  EVM/USDC payouts without his confirmation).
- Post-contest bug bounties: Watsons who audited a protocol can also earn
  from its live bug bounty afterwards — same known/fixed check applies.

## Using Sherlock WITHOUT a stake (the free parts)

- Read public contest reports at github.com/sherlock-audit and
  github.com/sherlock-protocol/sherlock-reports — free, no account needed.
  Use them as prior-art / known-issues checks before submitting the SAME
  finding anywhere else (Cantina, CodeHawks, direct email).
- Study judging issues in old judging repos to learn what Sherlock judges
  accept and reject — this calibrates severity arguments on every platform.
- Track live contests at audits.sherlock.xyz so Sky knows when a stake
  becomes worth funding (only he decides that).

## Pre-submission checklist (run every time)

1. Is the stake funded and the contest entry confirmed by Sky? (If no, STOP
   — draft only, no submission.)
2. Is the code in scope, at the pinned commit? (If no, stop.)
3. Is the finding on the known-issues list or in a prior Sherlock report?
   (If yes, stop — do not submit.)
4. Is it already fixed in the live deployed code? (If yes, stop.)
5. Does a coded PoC reproduce it against the pinned commit? (If no, do not
   submit a High/Medium.)
6. Is the severity argued from exploitability + impact, preconditions honest?
7. Is the draft plain text, no `#`, no `*`, with file:line references?
8. Has Sky approved this specific submission? (If no, draft only.)

## Common traps

- Entering without understanding the stake is at risk — poor submissions
  lose staking rewards.
- Submitting a finding the contest README already lists as known.
- Arguing severity from "could" and "if" without a concrete path — Sherlock
  judges invalidate speculative findings fast.
- Duplicating a finding another Watson already filed clearly — the split
  makes it nearly worthless; find the angle they missed instead.
- Forgetting the coverage angle: Sherlock-audited protocols carry exploit
  coverage, so a missed bug found AFTER launch may still pay via the live
  bug bounty — but only if it is genuinely new and in scope.

## Sources

- Sherlock homepage and researcher pages: https://sherlock.xyz
- Live contests: https://audits.sherlock.xyz
- Public contest reports: https://github.com/sherlock-audit and
  https://github.com/sherlock-protocol/sherlock-reports
- Platform figures (200+ Watsons, $50K-$300K pools, 7-30 day windows,
  $2M coverage, Euler $4.5M) verified against smartcontractaudit.com and
  smartcontractshacking.com Sherlock pages, Nov 2026.
- Stake figure (250 USDC) from Sky's own standing record: Sherlock route
  blocked by the 250 USDC stake, unaffordable at ~$0.22-$5.86 liquid.
