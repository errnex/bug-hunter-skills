---
name: immunefi
description: Immunefi Web3 bug bounty platform — submit smart-contract findings, 10 percent rule, severity classification V2.2/V2.3, PoC on local fork, KYC-at-payout rules, Lido and Olympus program facts. Use when auditing a smart contract for an Immunefi program, drafting an Immunefi report, or checking whether an Immunefi program passes Sky's FREE-only gate.
---

# Immunefi — Web3 Bug Bounty Platform

Immunefi is the largest Web3 bug bounty platform. It protects over $190
billion in user funds across 330+ projects (Chainlink, SushiSwap, MakerDAO,
Wormhole, Compound, Lido, Olympus and others). Programs live at
immunefi.com/bug-bounty/<program>/ with Information, Scope and Resources
tabs and a "Submit a Bug" button. Reports go through the platform, never by
email, unless a program page says otherwise.

## 1. How the platform works

- Each program is a standalone bounty with its own scope, assets, rules,
  reward table and KYC badge. There is no platform-wide reward table.
- Programs are tagged: "Triaged by Immunefi" (Immunefi staff triage) or
  triaged by the project team. Some enable arbitration (Immunefi mediates
  disputes over severity or reward).
- Payouts are usually in USDC on Ethereum, denominated in USD. Some programs
  pay in their native token (example: Compound DAO pays in COMP, priced as
  the average of CoinMarketCap and CoinGecko at submission time). The payout
  asset is stated on the program page under Rewards.
- Vault programs exist (funds locked in a vault contract). Read the program
  page before assuming a normal payout flow.
- Safe harbor: most programs include safe-harbor language for good-faith
  testing inside scope. Staying inside scope is what protects you. Testing
  outside scope, attacking mainnet with real funds, or withholding a bug
  voids protection and the reward.

## 2. Severity classification (Immunefi Vulnerability Severity Classification System V2.2 / V2.3)

Five levels, with SEPARATE scales for smart contracts, websites/apps, and
blockchains/DLTs. Do not apply a web severity table to a contract bug.

- Critical: direct theft or permanent freezing of funds, minting, governance
  takeover, or any impact that drains a material share of TVL.
- High: theft or freezing of a smaller amount, theft of unclaimed yield or
  royalties, temporary freezing with a recovery path.
- Medium: fund loss under restrictive conditions, griefing, or a bug that
  needs a privileged or unlikely precondition.
- Low: minor fund impact, or a bug with negligible economic effect.
- Informational / None: no direct impact. Usually unpaid.

Always quote the program's own "Impacts in Scope" table when you assign a
severity. The table, not your gut feeling, decides the level.

## 3. The 10 percent rule (Critical and High smart-contract rewards)

This is the core Immunefi reward mechanic. Learn it cold.

- Critical smart-contract reward = 10 percent of the funds directly
  affected, capped at the program maximum.
- There is also a MINIMUM reward for Critical, set to discourage researchers
  from withholding a bug. Examples from live programs:
  - Compound Finance: max $1,000,000, min $50,000.
  - 1inch Smart Contracts: max $500,000, min $30,000.
  - Folks Finance: max $100,000, min $25,000.
  - Immunefi's own program: max $50,000, min $10,000.
- High vulnerabilities about theft or permanent freezing of unclaimed yield
  or royalties are paid as a range based on 100 percent of funds at risk,
  capped at the High maximum (example 1inch: $10,000 to $30,000).
- Medium and Low are flat amounts per program (examples: Medium $5,000,
  Low $1,000 on several programs, but always read the program table).
- Funds at risk are calculated at the TIME AND DATE the report is submitted.
- Time-decay rule used by several programs: the first attack counts at 100
  percent, then the amount is reduced by 50 percent for every 24 hours the
  attack needs for subsequent attacks, rounded down. A slow multi-day drain
  pays less than an instant one. Put the fastest realistic attack path in
  the PoC.
- Website and application Criticals pay a flat amount (example: $10,000
  only if the impact is a direct loss of funds with an attack that needs no
  user action at all; other Critical web impacts pay $5,000 on that program).
  Web rewards do NOT use the 10 percent rule.

## 4. Primacy of Impact vs Primacy of Rules

Every program declares one of these. It changes what you can report.

- Primacy of Impact: impact is prioritized over the asset list. You are
  encouraged to report any bug with an in-scope IMPACT even if the affected
  asset is not listed in scope. All other impacts fall under Primacy of
  Rules.
- Primacy of Rules: the program runs strictly under the terms on its page.
  An out-of-scope asset means out of scope, whatever the impact.
- Check which one the program uses before you spend hours on an asset that
  is not listed. It is stated in the Rewards section of the program page.

## 5. PoC requirement (hard gate)

- A PoC is REQUIRED for smart-contract reports. Explanations and statements
  are not accepted as a PoC. Code is required.
- A smart-contract report also needs a suggested fix to be eligible.
- PoC must run on a LOCAL FORK or a testnet. Mainnet testing is prohibited
  on the programs Sky hunts (Lido, Olympus). Never execute an exploit
  against live mainnet contracts with real funds. Use Foundry or Hardhat
  fork mode pinned at a stated block number.
- A good PoC: one command to run, deterministic, prints the stolen or frozen
  amount in token units and USD at submission time, and states the block
  number it was pinned at.
- A model or simulation PoC never establishes impact. Only execution
  against the real deployed bytecode does. If the PoC runs against a
  re-implementation instead of the deployed code, say so plainly and do not
  claim a severity until a fork PoC passes.

## 6. KYC and eligibility

- KYC is PER PROGRAM, not platform-wide. The program page shows a "KYC
  required" badge when it applies. When KYC applies, Immunefi asks for full
  name, date of birth, proof of address (for example a redacted bank
  statement showing the address) and passport details (date of birth,
  issuing country, passport number) before payout.
- KYC is a PAYOUT gate, not a submission gate, on the programs Sky hunts.
  You can submit without KYC and complete it only if the report is accepted.
- Researchers in OFAC-restricted countries and persons named under UNSC
  resolutions are ineligible for rewards. Some programs also require a
  certification that you are not acting for a sanctioned person or entity.
- Sky's payout address for Immunefi (EVM): use the address Sky confirms at
  submission time. The address on file from prior rounds is
  0xC008dFECd3Bb35B0aa4d1240f691Ed17Bd8a8Fb3. Never invent or substitute a
  payout address.

## 7. Scope and asset rules

- Read the Scope tab in full before touching anything: contracts, chains,
  repos and commit hashes, websites, and the explicit out-of-scope list.
- Only the latest tag or release is in scope on most programs. A bug in an
  old deployment is usually out of scope.
- Known issues kill reports. Check before auditing: the program's audit
  reports (many programs link an audits GitHub repo and state that any
  vulnerability listed there is ineligible), the repo CHANGELOG and README
  known-issues section, prior Immunefi/Cantina/Sherlock/Code4rena contest
  findings, and public disclosures. A duplicate of a known issue pays $0.
- Bugs that require an attack involving one or more OTHER protocols (for
  example flash loans from a margin protocol, or manipulating an external
  oracle) are restricted or excluded on several programs. Read the program's
  restrictions section.
- Bugs related solely to external incentives distributed on top of the
  protocol are downgraded one severity level on some programs.

## 8. How to submit (step by step)

1. Create an Immunefi account (Sky only — Bang never creates accounts).
   Connect a wallet for payout; no fee to register.
2. Open the program page and read Information, Scope and Resources end to
   end. Confirm the program is live, the reward table, the KYC badge, and
   the Primacy rule.
3. Run the pre-submit known/fixed check (section 10). If the finding is
   known, fixed, or out of scope, stop. Do not submit.
4. Click "Submit a Bug" on the program page.
5. Fill the form: title, vulnerability details (root cause, affected
   contract and function, bug class), severity, PoC (code, runnable, with
   run command and pinned block), impact (quantified: funds at risk in USD
   at submission time, attack cost, repeatability), suggested fix, and
   payout wallet address.
6. Submit once. Do not submit duplicates. If triage asks for more
   information, answer in the same report thread.
7. Wait for triage. There is no platform-wide SLA; Immunefi-triaged programs
   usually respond within days, project-triaged programs vary. Never
   re-submit the same bug to chase a reply.

## 9. Report template (plain text for Sky's draft)

Sky's rule: drafts are plain text, with no # and no * characters anywhere.
Write the draft that way from the first line. The template below follows
that rule.

Title: Bug class in contract and function allows attacker to quantified impact

Summary: One paragraph. Root cause, affected contract and function,
economic impact in USD at submission time, attack cost in gas, whether the
attack is repeatable.

Vulnerability details:
Contract: exact contract name and address
Function: exact function name
Bug class: for example accounting desynchronization, access control,
oracle manipulation, reentrancy
Severity: Critical, High, Medium or Low, with the Impacts in Scope row it
matches

Root cause: exact code snippet of the vulnerable code, with the faulty lines
explained in plain words.

Proof of Concept: runnable Foundry test, run command, pinned block number,
and the observed output (amount stolen or frozen, in token units and USD).
State clearly whether it ran against deployed bytecode on a local fork.

Impact: quantified funds at risk, who loses, how fast, attack cost,
repeatability. No theoretical impact, only what the PoC demonstrated.

Recommended fix: specific code change, before and after if short.

References: audit report links, prior disclosures checked, program scope
rows relied on.

## 10. Sky-specific program facts (verified in prior rounds)

- Lido (Immunefi): FREE, no KYC, PoC required on local fork, mainnet
  testing prohibited. Critical maximum $2,000,000. Audited round 17,
  2026-11-21, 0 Critical or High. Do not re-audit without new facts (new
  release, new scope, new advisory, new disclosure).
- Olympus / MonoCooler (Immunefi): FREE, no KYC, Critical maximum
  $3,330,000, the largest verified-free program in the hunt. Audited round
  15, 2026-11-21, 0 Critical or High. Re-audit only on a rate hike, new
  scope, new commit, new disclosure, or gOHM below the LLTV threshold
  recorded in the round 15 report.
- General gate: a program is FREE only when submission cost, reputation
  minimum, KYC requirement and PoC requirement are all confirmed from the
  program page itself. Never call a program free from the rewards section
  alone.

## 11. Sky standing rules (apply to every Immunefi report)

- FREE only. Any submission fee or stake is a hard stop unless Sky
  explicitly approves the cost.
- Bang never submits a report in Sky's name. Prepare the draft and the PoC
  bundle; Sky submits.
- Before ANY submission, check first whether the finding is already known
  or fixed: known-issues list, prior audits, public disclosures, live
  deployed state at the pinned block. Never submit a known or fixed finding.
- No destructive testing. No mainnet exploit execution with real funds.
- Drafts are plain text with no # and no * characters.

## 12. Common traps

- Quoting the program maximum as the expected reward. The 10 percent rule
  decides the reward; the maximum is a cap, not a promise.
- Submitting a PoC that runs against your own re-implementation instead of
  deployed bytecode, then claiming Critical. Fork PoC or it does not count.
- Auditing an old tag. Only the latest release is in scope on most programs.
- Missing the Primacy rule and reporting an out-of-scope asset on a
  Primacy of Rules program.
- Reporting a bug already listed in the program's audits repo. That is an
  automatic ineligible.
- Forgetting the time-decay rule: a drain that needs days of repeated
  attacks pays far less than the headline TVL suggests.
- Withholding a bug to negotiate. The minimum-reward rule exists to stop
  this; withholding still voids eligibility on most programs.

## Sources

- Immunefi program pages (immunefi.com/bug-bounty/<program>/information):
  Immunefi own program, Compound Finance, 1inch Smart Contracts, Folks
  Finance — rewards tables, 10 percent rule text, KYC sections, Primacy of
  Impact vs Primacy of Rules, severity system V2.2/V2.3. Read live
  2026-11-23.
- Sky hunt memory: Lido round 17 and Olympus round 15 reports, 2026-11-21.
