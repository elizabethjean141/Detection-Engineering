# MITRE Mapping — BlackCat Insider Negotiator Case

## Note on Framework Choice

This case has **two simultaneous threat actors**:

1. **The external adversary** (BlackCat/ALPHV operators) — technical attack, fits MITRE ATT&CK
2. **The insider co-conspirators** (Martino, Martin, Goldberg) — insider threat, fits the **MITRE Insider Threat TTP Knowledge Base**

We document both below because a real SOC analyzing this case would need both views.

---

## External Adversary: BlackCat/ALPHV (ATT&CK)

Representative techniques from BlackCat's documented TTP profile (see [MITRE ATT&CK Software S1068](https://attack.mitre.org/software/S1068/)):

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Valid Accounts | [T1078](https://attack.mitre.org/techniques/T1078/) |
| Initial Access | External Remote Services | [T1133](https://attack.mitre.org/techniques/T1133/) |
| Execution | Command and Scripting Interpreter: PowerShell | [T1059.001](https://attack.mitre.org/techniques/T1059/001/) |
| Persistence | Create Account: Local Account | [T1136.001](https://attack.mitre.org/techniques/T1136/001/) |
| Privilege Escalation | Abuse Elevation Control Mechanism | [T1548](https://attack.mitre.org/techniques/T1548/) |
| Defense Evasion | Impair Defenses: Disable or Modify Tools | [T1562.001](https://attack.mitre.org/techniques/T1562/001/) |
| Defense Evasion | Indicator Removal | [T1070](https://attack.mitre.org/techniques/T1070/) |
| Credential Access | OS Credential Dumping: LSASS Memory | [T1003.001](https://attack.mitre.org/techniques/T1003/001/) |
| Discovery | System Information Discovery | [T1082](https://attack.mitre.org/techniques/T1082/) |
| Lateral Movement | Remote Services | [T1021](https://attack.mitre.org/techniques/T1021/) |
| Collection | Data from Local System | [T1005](https://attack.mitre.org/techniques/T1005/) |
| Exfiltration | Exfiltration Over Web Service | [T1567](https://attack.mitre.org/techniques/T1567/) |
| Impact | Data Encrypted for Impact | [T1486](https://attack.mitre.org/techniques/T1486/) |
| Impact | Inhibit System Recovery | [T1490](https://attack.mitre.org/techniques/T1490/) |
| Impact | Financial Theft | [T1657](https://attack.mitre.org/techniques/T1657/) |

---

## Insider Threat Framework Mapping

The MITRE Insider Threat TTP Knowledge Base is a newer framework (2022+) that classifies insider actions. The trio's conduct maps cleanly:

### Martino / Martin (DigitalMint Negotiators)

| Behavior | Insider Threat Category |
|---|---|
| Sharing confidential client negotiation positions with adversaries | **Information disclosure** via authorized access |
| Sharing insurance policy limits with adversaries | **Information disclosure** via authorized access |
| Coordinating extended ransom deadlines to extract maximum payment | **Sabotage of legitimate business process** while appearing to execute it |
| Operating as BlackCat affiliates directly | **Collusion with external threat actor** |
| Receiving payment for betrayed information | **Financial motivation** |

### Goldberg (Sygnia IR Manager)

| Behavior | Insider Threat Category |
|---|---|
| Deploying ransomware against organizations he was professionally obligated to defend against | **Privileged access misuse** |
| Combining employer-provided tools/knowledge with criminal activity | **Dual-use knowledge exploitation** |

### Structural Enablers

| Condition | Role in Enabling the Conspiracy |
|---|---|
| Industry specialization | Small pool of experienced negotiators → hard to cross-check their work |
| Client desperation | Victim under pressure doesn't verify every negotiator decision |
| No independent negotiation oversight | DigitalMint's internal controls did not detect leaks |
| Ransomware affiliate model | BlackCat gave any paying affiliate access, no background vetting |

---

## Where Traditional Detection Fails

Detection engineering exists to find adversaries who must **violate trust boundaries** to act. An insider already *has* all the trust boundaries open to them. Specifically:

- **No unusual process creation** — Martino used a chat interface he was authorized to use
- **No unusual file access** — he was supposed to see the insurance documents and negotiation chats
- **No unusual network traffic** — communication with BlackCat was through channels he was supposed to use during negotiation
- **No unusual login times** — he worked during business hours
- **No unusual privilege escalation** — he had the privileges his job required

The ATT&CK framework wouldn't have helped. The detection engineering playbook we normally use assumes the adversary is outside the perimeter. Here, the adversary **was the perimeter**.

---

## What Would Have Worked

Controls that could have caught this are governance-layer, not endpoint-layer:

1. **Multi-party negotiation approval** — No single negotiator sees full victim context (separation of duties)
2. **Periodic negotiation outcome audits** — Statistical analysis of ransom-paid-vs-demand ratios per negotiator. An outlier pattern (one negotiator consistently closing at max demand) is a flag.
3. **Counter-intelligence on affiliate chatter** — Monitoring BlackCat affiliate forums for leaked client data, matched against active client list
4. **Employee financial background monitoring** — Not mass surveillance, but consistency checks for negotiators handling multi-million-dollar transactions
5. **Whistleblower channels inside the negotiator industry**

None of these are technical detection engineering. All of them are insider threat program design. This is why defenders benefit from studying cases like this — the failure mode is outside the tools we usually reach for.
