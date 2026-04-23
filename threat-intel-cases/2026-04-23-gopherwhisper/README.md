# GopherWhisper APT — China-Aligned Go Backdoors Targeting Mongolian Government

> **Case opened:** 2026-04-23
> **Analyst:** Elizabeth ([Threat Huntress Lab](https://github.com/elizabethjean141/Detection-Engineering))
> **Primary source:** [ESET We Live Security — "GopherWhisper: A burrow full of malware"](https://www.welivesecurity.com/en/eset-research/gopherwhisper-burrow-full-malware/)
> **Corroborating sources:** [The Hacker News](https://thehackernews.com/2026/04/china-linked-gopherwhisper-infects-12.html), [GlobeNewswire via ESET](https://www.eset.com/)
> **TLP:** AMBER · **PAP:** GREEN · **Severity:** High
> **Case type:** APT · Cloud Service Abuse · Government Targeting

---

## Summary

ESET has disclosed a previously undocumented China-aligned APT group tracked as **GopherWhisper**. The group targeted Mongolian government institutions starting in January 2025. Approximately 12 systems at one named Mongolian governmental entity were confirmed compromised; dozens of additional victims are inferred from captured C&C traffic but are not publicly attributed.

The toolset is notable for two reasons:

1. **Language choice.** Most implants are written in Go — an uncommon but increasingly popular language for espionage malware.
2. **C&C channel choice.** The group abuses legitimate cloud services — **Slack, Discord, Microsoft 365 Outlook (Graph API), and file.io** — for command-and-control and data exfiltration.

Attribution to China is based on working-hour analysis of captured C&C messages (8 AM – 5 PM China Standard Time) and Slack metadata indicating the threat actor's account was configured for CST.

---

## Toolset

| Tool | Language | Role |
|---|---|---|
| JabGopher | Go | Injector — executes LaxGopher (`whisper.dll`) |
| LaxGopher | Go | Primary backdoor — Slack C2, runs `cmd.exe`, downloads additional payloads |
| CompactGopher | Go | File collection — filters by extension, ZIPs, AES-CFB-128 encrypts, exfils via file.io |
| RatGopher | Go | Backdoor — Discord C2, file upload/download via file.io |
| SSLORDoor | C++ | Backdoor — OpenSSL raw socket over port 443, cmd.exe execution |
| FriendDelivery | DLL | Loader / injector for BoxOfFriends |
| BoxOfFriends | Go | Backdoor — Microsoft Graph API for C2 via Outlook drafts |

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Execution | Command and Scripting Interpreter: Windows Command Shell | [T1059.003](https://attack.mitre.org/techniques/T1059/003/) |
| Execution | Native API | [T1106](https://attack.mitre.org/techniques/T1106/) |
| Defense Evasion | Process Injection | [T1055](https://attack.mitre.org/techniques/T1055/) |
| Command and Control | Application Layer Protocol: Web Protocols | [T1071.001](https://attack.mitre.org/techniques/T1071/001/) |
| Command and Control | Web Service: Bidirectional Communication | [T1102.002](https://attack.mitre.org/techniques/T1102/002/) |
| Command and Control | Web Service: One-Way Communication | [T1102.001](https://attack.mitre.org/techniques/T1102/001/) |
| Collection | Data from Local System | [T1005](https://attack.mitre.org/techniques/T1005/) |
| Collection | Archive Collected Data: Archive via Utility | [T1560.001](https://attack.mitre.org/techniques/T1560/001/) |
| Collection | Archive Collected Data: Archive via Custom Method | [T1560.003](https://attack.mitre.org/techniques/T1560/003/) |
| Exfiltration | Exfiltration Over Web Service | [T1567](https://attack.mitre.org/techniques/T1567/) |
| Exfiltration | Exfiltration to Cloud Storage | [T1567.002](https://attack.mitre.org/techniques/T1567/002/) |

---

## VirusTotal Enrichment

The following observables were checked against VirusTotal using the lab's Wazuh→VirusTotal integration (configured and tested 2026-04-23):

| Observable | VT Verdict | Notes |
|---|---|---|
| `discord.com` | Legitimate (Majestic rank 133, Cisco Umbrella 3329, 25+ year-old domain) | Abused for C2 by RatGopher |
| `slack.com` | Legitimate (0 malicious / 64 harmless vendor verdicts) | Abused for C2 by LaxGopher |
| `file.io` | Legitimate (valid Google-signed TLS certificate) | Abused for exfiltration by CompactGopher and RatGopher |
| `barrantaya.1010@outlook.com` | Not directly scannable via VT (email reputation not supported) | Attacker-controlled M365 account, created 2024-07-11 for BoxOfFriends C2 |
| `whisper.dll` (filename search) | No records returned | LaxGopher sample not yet in VT's public dataset — ESET likely retaining for subscribers |

### Analyst Conclusion on Enrichment

All observed C2 and exfiltration destinations are **legitimate services being abused**. VirusTotal enrichment confirms that hash-based or reputation-based detection of the C&C channels is not viable. Detection must rely on behavioral and policy signals, not signatures. This is a deliberate design choice by the threat actor — blending into normal traffic is the primary evasion strategy.

---

## Detection Guidance

Signature-based detection is not available for the known payloads at time of writing. Proposed behavioral detection approaches:

### 1. Endpoint Policy Enforcement
Government and other sensitive workstations should not initiate outbound connections to Discord, Slack, or file.io. An allow-list traffic policy would cause these C2 channels to fail silently without needing to "detect" anything.

### 2. Sysmon Event ID 3 (Network Connection) Correlation
On endpoints where Slack/Discord/file.io traffic is not expected, flag any outbound connection to those destinations. Especially suspicious when the parent process is `whisper.dll`, `cmd.exe`, or any DLL loaded from a non-standard path.

### 3. Microsoft Graph API Abuse Monitoring
Unusual creation of draft emails via Graph API tokens tied to unknown accounts (e.g., `barrantaya.1010@outlook.com`) is a strong signal. Requires M365 Unified Audit Log ingestion.

### 4. LOLBin Parent-Child Correlation
`cmd.exe` spawned by a non-standard parent — particularly a DLL host or recently-loaded injector — correlates with LaxGopher and SSLORDoor execution patterns.

### 5. Hash Blocking (Pending)
Once ESET publishes IOC lists (typically 30-90 days after initial disclosure), add known hashes to Wazuh FIM and endpoint blocklists.

---

## Analyst Note — Patient Zero

ESET does not describe how GopherWhisper obtains initial access. That gap is significant. Every downstream tool, every backdoor, every piece of Go malware, is useless without a foothold. The foothold is how this campaign starts, and it is the single most important detail missing from the public report.

For defenders, initial access vectors for China-aligned APTs targeting government typically include spear-phishing with malicious attachments, exploitation of internet-facing services, and supply-chain compromises. **Detection investment for this threat profile should focus upstream on phishing and perimeter service hygiene, not downstream on the post-exploitation Go backdoors.**

By the time `whisper.dll` loads, the attacker is already inside.

---

## Case Status

Intake complete. No evidence of local compromise in the Threat Huntress Lab environment. Case retained as threat intel reference for future detection rule development.

### Follow-up Tasks

- [ ] Review last 30 days of Wazuh alerts for `cmd.exe` spawned by DLL loaders on non-administrator endpoints
- [ ] Monitor ESET's IOC publication feed for `whisper.dll` hashes (30-90 day window)
- [ ] Review lab outbound firewall policy for Discord/Slack/file.io reachability
- [ ] Evaluate M365 Unified Audit Log ingestion for BoxOfFriends-style draft email C2

---

## References

- ESET We Live Security — https://www.welivesecurity.com/en/eset-research/gopherwhisper-burrow-full-malware/
- The Hacker News — https://thehackernews.com/2026/04/china-linked-gopherwhisper-infects-12.html
- MITRE ATT&CK — https://attack.mitre.org/
