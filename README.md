# Bug Hunter Skills

Skill pack for Sky's Hermes agent (Discord bot on the VPS). 19 skills for the FREE-only, PoC-proven bug bounty hunt.

## Install (Hermes)

In a Discord thread or terminal where Hermes runs:

```
hermes skills install <raw SKILL.md URL>
```

Use one URL per skill from the list below. Each skill is a single SKILL.md file. After installing, restart the gateway so the new skills load.

## Skills

### Core

- security-bounty-hunter — main hunter playbook. Practical vulnerability discovery for bounty submission, FREE-only gate, PoC-proven or it does not ship.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-security-bounty-hunter/SKILL.md
- 0xsteph-bug-bounty — bug bounty methodology, target prioritization, report writing (adapted from 0xSteph/pentest-ai-agents, plus Sky's house rules).
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-0xsteph-bug-bounty/SKILL.md
- 0xsteph-web-hunter — web app pentest: fuzzing, SQLi, hidden endpoints, parameter discovery.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-0xsteph-web-hunter/SKILL.md
- 0xsteph-code-auditor — secure code review: static analysis, injection, auth, secrets, SAST tooling.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-0xsteph-code-auditor/SKILL.md
- 0xsteph-report-generator — pentest report writing, findings, executive summary.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-0xsteph-report-generator/SKILL.md
- 0xsteph-privesc-advisor — privilege escalation techniques, Linux/Windows, container escape.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-0xsteph-privesc-advisor/SKILL.md

### Platforms

- immunefi — Immunefi Web3 bug bounty: smart-contract findings, 10 percent rule, severity V2.2/V2.3, PoC on local fork, KYC-at-payout rules, pay-to-submit gate check via raw HTML.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-immunefi/SKILL.md
- intigriti — Intigriti: program tiers, severity, CVSS, validation SLAs, Onfido IDV, @intigriti.me testing rules.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-intigriti/SKILL.md
- yeswehack — YesWeHack: credits per submission, MangoPay KYC gate, CVSS form scoring, report structure.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-yeswehack/SKILL.md
- cantina-bounty — Cantina audit competitions: lifecycle, severity, points, judging, escalation.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-cantina-bounty/SKILL.md
- codehawks-bounty — CodeHawks (Cyfrin) audit contests and First Flights practice track.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-codehawks-bounty/SKILL.md
- code4rena-bounty — Code4rena archive. CLOSED May 2026. Use for its public report archive and Immunefi migration, not new contests.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-code4rena-bounty/SKILL.md
- sherlock-bounty — Sherlock audit contests. NOT-FREE for Sky (250 USDC stake gate). Reference only.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-sherlock-bounty/SKILL.md

### Specialty

- azure-pentest — Microsoft Azure and Entra ID penetration testing.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-azure-pentest/SKILL.md
- gcp-pentest — Google Cloud Platform penetration testing.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-gcp-pentest/SKILL.md
- bridge-exploit-forensics — cross-chain bridge exploit forensics: LayerZero, Wormhole, Axelar, CCIP, Ronin, Nomad.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-bridge-exploit-forensics/SKILL.md
- browser-extension-audit — browser extension security audit (Chrome MV3 / Firefox).
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-browser-extension-audit/SKILL.md
- defi-amm-security — DeFi AMM security checklist: Solidity pools, LP vaults, swap flows, oracle and admin controls.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-defi-amm-security/SKILL.md
- wallet-security — crypto wallet audit for bounty hunting: seed and private-key handling, blind signing, unlimited approvals, Permit/Permit2, EIP-7702 delegation, malicious dApps, approval phishing.
  https://raw.githubusercontent.com/errnex/bug-hunter-skills/main/local-wallet-security/SKILL.md

## House rules (apply on top of every skill)

- FREE only. No submission fee, no stake, no paid program.
- PoC-proven or it does not ship. Model or simulation alone is never proof.
- Never submit in Sky's name on identity-gated platforms. Email-route reports go from Sky's Gmail.
- Before any submission, check whether the finding is already known or fixed.
- No destructive testing.
- Drafts are plain text: no hash, no asterisk, no underscore, no emoticons.
