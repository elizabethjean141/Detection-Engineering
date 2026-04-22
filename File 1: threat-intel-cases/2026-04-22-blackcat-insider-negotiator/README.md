# BlackCat Insider Negotiator Case — DigitalMint / Sygnia Conspiracy

> **Case opened:** 2026-04-22
> **Analyst:** Elizabeth ([Threat Huntress Lab](https://github.com/elizabethjean141/Detection-Engineering))
> **Source:** [DOJ Press Release](https://www.justice.gov/opa/pr/florida-man-working-ransomware-negotiator-pleads-guilty-conspiracy-deploy-ransomware-and), [BleepingComputer](https://www.bleepingcomputer.com/news/security/former-ransomware-negotiator-pleads-guilty-to-blackcat-attacks/), [CyberScoop](https://cyberscoop.com/digitalmint-ransomware-negotiator-angelo-martino-guilty-plea/)
> **Case type:** Insider Threat · Vendor Risk · Trust Abuse
> **TLP:** GREEN · **PAP:** GREEN · **Severity:** High

---

## Executive Summary

Three cybersecurity professionals who were **paid to help ransomware victims** were instead feeding information back to the attackers. From April to November 2023, **Angelo Martino** and **Kevin Tyler Martin** (both ransomware negotiators at DigitalMint) conspired with **Ryan Clifford Goldberg** (an incident response manager at Sygnia) to:

1. Share victim negotiation positions and insurance limits with BlackCat/ALPHV affiliates
2. Operate as BlackCat affiliates themselves — paying the ransomware operators a 20% cut for access to the platform
3. Split ransom proceeds and launder the funds

All three pled guilty. Martino was sentenced to face up to 20 years. Authorities seized approximately **$10 million in assets** from Martino alone (cryptocurrency, vehicles, a food truck, and a luxury fishing boat).

Across at least five organizations, the trio helped extort a total exceeding **$75 million**, including a single nonprofit that paid $26.79M and a financial services firm that paid $25.66M.

This is not a malware case. This is a **governance, trust, and insider threat case** — and the most important case for defenders to study this quarter.

---

## Why This Case Matters to Defenders

Most threat intel reports describe external adversaries. This case describes a failure mode most organizations don't plan for:

**The specialists you hire to protect you can betray you.**

The entire negotiation process depends on the victim trusting their negotiator completely — they give the negotiator their insurance limits, their bottom-line numbers, their executives' urgency. When that trust is misplaced, the organization pays the maximum possible ransom without ever knowing the inside was leaking.

DigitalMint is not accused of any wrongdoing. Their CEO has stated the company terminated both Martin and Martino immediately upon learning of the FBI investigation. But the fact remains — the company's internal controls did not catch the betrayal. It was caught by law enforcement investigating from outside.

---

## Key Facts

| Item | Detail |
|---|---|
| Subjects | Angelo Martino (41, FL), Kevin Tyler Martin (28, TX), Ryan Clifford Goldberg (33, GA) |
| Employers | DigitalMint (Martino, Martin), Sygnia (Goldberg) |
| Ransomware | BlackCat / ALPHV (RaaS, 20% cut to operators) |
| Activity period | April 2023 – November 2023 (direct attacks); Apr 2023 – Apr 2025 (affiliate activity) |
| Victim count | At least 5 organizations (financial services, nonprofit, law firms, school districts, medical) |
| Largest victim payments | $26.79M (nonprofit), $25.66M (financial services firm) |
| Total attributed extortion | ~$75M across ~10 attacks |
| Assets seized from Martino | ~$10M (crypto, vehicles, food truck, luxury fishing boat) |
| Maximum sentence | Up to 20 years each |
| Martino sentencing date | July 9, 2026 |

---

## Core Betrayal Pattern

Martino's role was to negotiate **down** the ransom on behalf of victims. Instead, he negotiated it **up** on behalf of the attackers.

From CyberScoop's reporting on his plea agreement chats:

> Martino told a BlackCat affiliate the company's insurance carrier "was only approving small accounts... Keep denying our offers and I will let you know once I find out the max they want to pay."

This is a direct, documented instance of a trusted fiduciary actively sabotaging their own client during the very transaction they were hired to execute. The victim company could see its own side of the chat with BlackCat. It could not see the parallel chat feeding its bottom-line figures to the attackers.

---

## Case Structure

This case folder contains:

- [`timeline.md`](./timeline.md) — chronological sequence of events
- [`mitre-mapping.md`](./mitre-mapping.md) — ATT&CK and Insider Threat framework mapping
- [`lessons-learned.md`](./lessons-learned.md) — governance and detection takeaways

---

## Patient Zero Question

*For every threat intel case — how did someone notice first?*

The DOJ and FBI ran the investigation. They caught it from the outside. None of the victim companies caught it. DigitalMint itself didn't catch it until the FBI informed them they were investigating Martino in April 2025 — **two full years** after the first leaked negotiation.

For organizations: what internal controls could have caught a trusted negotiator leaking information? And more broadly — what does "insider threat" detection actually look like when the insider is authorized to see everything they're leaking?

See [`lessons-learned.md`](./lessons-learned.md) for the controls discussion.
