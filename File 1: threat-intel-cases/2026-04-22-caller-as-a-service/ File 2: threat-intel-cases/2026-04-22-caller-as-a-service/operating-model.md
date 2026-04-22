# Caller-as-a-Service — Operating Model

## Division of Labor

The ecosystem has distinct, specialized roles. A single attack involves multiple actors, each paid for their part, most of whom never speak to each other.

| Role | Responsibility | Example Output |
|---|---|---|
| **Breach operator** | Original compromise of upstream data source | Corporate customer database |
| **Log seller / data broker** | Cleans, formats, and resells victim lists | "10k US elderly with SSN last 4 — $500" |
| **Phishing kit builder** | Develops fake websites, MFA-prompt kits | Cloudflare-bypassing landing page |
| **Infrastructure operator** | Runs VoIP trunks, number spoofing, proxies | Spoofed caller ID infrastructure |
| **Scripter / social engineer** | Writes call scripts, urgency triggers | "IRS tax enforcement division" script |
| **Scam caller (human)** | Executes the live call | Actual voice interaction with victim |
| **Floor supervisor** | Quality control, real-time oversight | Screen-shares during active calls |
| **Money mule coordinator** | Converts stolen access into cashable assets | Gift cards, wires, crypto off-ramps |
| **Launderer** | Final conversion and distribution | Mixed crypto, cashed out |

**Each layer specializes.** A scam caller does not need to know how to breach a database. A breach operator does not need to know how to convince an elderly person to go to Walmart for gift cards. The labor is divided exactly as modern corporations divide labor.

---

## Recruitment

Recruitment ads on underground forums and chat channels read remarkably like legitimate sales job postings:

### Typical Requirements (per Flare's and Vectra AI's observations)

- **Native English proficiency** — to match targets in US, UK, Canada, AU/NZ
- **OPSEC awareness** — use of VPNs, no personal info leakage, no photos, etc.
- **Prior social engineering experience** — measurable conversion rates on past gigs
- **Availability for screen-share supervision** — live oversight during calls
- **Clean interview** — recruiters vet candidates before accepting them

### Credibility Signals

Underground recruitment posts often include screenshots of cryptocurrency wallet balances (Flare cites an example of ~$475,000) as "proof of profit" — functionally the same as a legitimate company displaying financial statements to attract employees. Whether the wallet is authentic or staged is beside the point; the signal reduces skepticism.

---

## Compensation Models

Flare identifies three models currently in use:

### Fixed Rate
- Flat payment per successful call (e.g., **$1,000/call**)
- Attractive to new callers wanting predictable income
- Lower ceiling but lower risk of non-payment

### Percentage / Success-Based
- Caller takes a percentage of extracted funds
- Higher percentages for higher payouts (incentivizes targeting wealthier victims)
- Higher ceiling, higher variance, longer time-to-payment

### Hybrid
- Combined flat + percentage
- Operator retains more control over downstream monetization
- Some payouts are **delayed** or **conditioned** because successful social engineering does not always translate immediately into cashable funds

This matches the **Scattered Spider / ShinyHunters alliance** model documented by Vectra AI, where vishing operators are paid $500–$1,000 per successful social engineering call.

---

## Real-Time Supervision

The most striking shift from traditional scam calls: **live supervision via screen-share during calls**.

Purpose:
- **Script adherence** — ensuring callers don't deviate from tested-effective language
- **Conversion optimization** — a supervisor can intervene mid-call if the victim starts to doubt
- **Quality control** — ejecting underperforming callers quickly
- **Internal fraud prevention** — preventing the caller from keeping funds for themselves rather than passing them up

This is structurally identical to a legitimate sales floor with a manager listening on calls. That's the point. **Industrialization means management.**

---

## Attack Lifecycle (End-to-End)

```
┌────────────────────┐
│  1. UPSTREAM DATA  │  ← Corporate breach (months earlier)
│      SOURCE        │    Data broker aggregation
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│ 2. VICTIM LIST     │  ← Sold / traded in underground
│    ACQUISITION     │    markets, enriched with context
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  3. SCRIPT &       │  ← Pretext crafted: IRS, bank,
│  PRETEXT PREP      │    tech support, grandchild emergency
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  4. CALLER ID      │  ← VoIP spoofing of trusted number
│     SPOOFING       │    (bank, gov agency, family)
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  5. LIVE CALL      │  ← Caller with supervisor on
│  (SUPERVISED)      │    screen-share; emotional pressure
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  6. CONVERSION     │  ← Victim acts: gives credential,
│                    │    sends money, installs RAT
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  7. MONETIZATION   │  ← Mule network, gift cards,
│                    │    wires, crypto off-ramp
└─────────┬──────────┘
          │
          ▼
┌────────────────────┐
│  8. LAUNDER        │  ← Mixer, chain hopping,
│                    │    cash out to fiat
└────────────────────┘
```

Only stage 5 is visible to the victim. Everything else is invisible, distributed, and designed to be resilient to disruption.

---

## Why Law Enforcement Disruption Is Hard

The Flare analysis correctly notes: *"Removing individual callers has limited impact, as critical components (victim data, operators, and monetization channels) are distributed and resilient."*

If law enforcement arrests a caller, the operators hire another caller that afternoon. If law enforcement seizes a VoIP infrastructure operator, another one is online within days. The only durable intervention is at the **victim data supply** (upstream breach prevention and notification) and the **monetization rail** (crypto off-ramp and banking fraud controls).
