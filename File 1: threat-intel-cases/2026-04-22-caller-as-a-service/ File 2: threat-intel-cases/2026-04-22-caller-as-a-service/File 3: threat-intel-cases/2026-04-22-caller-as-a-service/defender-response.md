# Defender Response — What to Actually Do

Caller-as-a-Service attacks sit in a gap most security programs don't cover. The infrastructure side is out of scope for corporate SOCs (individuals are the targets, not companies), and the human side is out of scope for traditional detection engineering. This file tries to give concrete guidance anyway.

---

## For Enterprise Security Teams

Even if your company isn't directly the target, employees and customers are. These controls reduce blast radius when vishing hits.

### 1. Help-Desk Social Engineering Defenses

Scattered Spider's vishing success against enterprises came from calling the IT help desk and impersonating employees. Counter-controls:

- **Out-of-band identity verification** for all help-desk requests that change MFA or passwords (e.g., callback to a manager's known number)
- **Cooldown periods** on MFA resets — no same-day MFA device re-enrollment without additional review
- **Help-desk call recording and random audit** — this is standard in financial services and should be standard in IT
- **Do not accept "I'm locked out" as a priority-bypass trigger** — urgency is the attacker's tool

### 2. Customer-Facing Call Center Controls

If your company runs a call center that receives customer calls:

- **Voice biometrics** with liveness checking
- **Behavioral anomaly detection** on account interactions (e.g., sudden change in transaction patterns correlating with recent inbound call)
- **Call duration and velocity thresholds** — attackers often rush to extract value before suspicion
- **Integration between IVR, agent desktop, and fraud systems** so signals combine

### 3. Breach Notification Speed

Every day a breach remains undisclosed is a day attackers get exclusive, still-fresh data. If your company has PII exposure:

- Notify customers fast
- Include specific warning language about likely vishing patterns (not just generic "be careful")
- Publish a **call-back verification number** customers can use to verify any communication claiming to be from you

### 4. Wazuh / SIEM Detection Opportunities (Narrow)

There are no rules that detect the call itself. But there are signals that correlate with vishing-driven follow-through:

| Signal | Detection Opportunity |
|---|---|
| Help-desk ticket creation immediately followed by MFA device change | Correlate helpdesk system events with identity provider events |
| User self-service password reset from new geographic location | IdP geo-anomaly rule |
| User installs remote-access tool (AnyDesk, TeamViewer, ScreenConnect) that isn't standard | Sysmon EID 1 + process name allowlist violation |
| New mailbox forwarding rule created shortly after help-desk interaction | M365 Unified Audit Log correlation |
| Sudden download of browser-saved credentials or password manager export | DLP content rule |

These are what I'd call **"echo" rules** — they don't detect the scam call, but they detect the actions a victim takes *because of* a scam call.

---

## For Individuals

### Universal Rules

1. **Never share passwords, MFA codes, or financial details on an inbound call**, regardless of how convincing the caller is
2. **Urgency is the signal** — any caller pressuring you to act immediately is almost certainly a scam
3. **When in doubt, hang up and call back** using a number you looked up independently (not a number the caller gave you)
4. **Enable MFA everywhere possible** — and prefer hardware keys or authenticator apps over SMS
5. **Freeze your credit** if you're not actively applying for anything — this blocks the most damaging downstream uses of stolen identity

### Red Flag Language

If you hear any of these, assume scam until proven otherwise:

- "Your account has been compromised" / "There's been suspicious activity"
- "We need to verify your identity" (followed by them asking for the verification)
- "Don't hang up" / "Stay on the line"
- "There's a warrant for your arrest" / "You owe back taxes"
- "Your grandchild is in jail and needs bail money"
- "We're from Microsoft / Apple / Amazon support" (these companies don't cold-call)
- "Go to Walmart and buy gift cards"
- "Install this app so we can help you"

### For Family Members Helping Elderly Relatives

- **Have the conversation before the scam happens**, not after
- **Give them a "panic number"** — if they get a suspicious call, tell them to call you before doing anything
- **Put transaction alerts on their accounts** that go to you as well
- **Know that shame prevents reporting** — victims often don't tell family because they feel foolish. Tell them ahead of time that scam calls fool professionals too, and there is nothing to be embarrassed about.

---

## For Security Writers / Awareness Programs

Move beyond "don't click suspicious links" as your awareness content. Vishing is a different attack vector and deserves different training. Consider:

- **Tabletop exercises** that simulate a vishing attempt to the IT help desk
- **Phishing simulations that include phone calls**, not just emails (with employee consent and a clear "this is training" reveal)
- **Short-form video content** showing what a scam call sounds like — most employees have never actually heard one up close
- **Survivor stories from the community** (with consent) — people who fell for a scam and are willing to share what convinced them

Awareness content that assumes the target is smart is more effective than awareness content that assumes the target is stupid. Scam calls work on smart people every day.
