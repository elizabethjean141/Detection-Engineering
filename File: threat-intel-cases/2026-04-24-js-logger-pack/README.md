# js-logger-pack — npm Dropper Abusing Hugging Face as CDN and Exfil Backend

> **Case opened:** 2026-04-24
> **Analyst:** Elizabeth ([Threat Huntress Lab](https://github.com/elizabethjean141/Detection-Engineering))
> **Primary source:** [JFrog Security Research — "js-logger-pack Operator Turns Hugging Face into a Malware CDN and Exfiltration Backend"](https://research.jfrog.com/post/hugging-face-exfil/) (Shavit Satou, April 23, 2026)
> **Corroborating sources:** [SafeDep (earlier campaign phase)](https://safedep.io/malicious-js-logger-pack-npm-stealer/), [Cybersecurity News](https://cybersecuritynews.com/malicious-npm-package-turns-hugging-face/), [ThreatFox IOC #1792046](https://threatfox.abuse.ch/ioc/1792046/)
> **TLP:** AMBER · **PAP:** GREEN · **Severity:** High
> **Case type:** Supply Chain · npm · AI Infrastructure Abuse

---

## Summary

A malicious npm package named `js-logger-pack` (versions up to `1.1.27`) acts as a supply-chain dropper that uses **Hugging Face** as both a malware distribution CDN and a live exfiltration backend. The package appears on npm as a legitimate-looking logging library — its `dist/index.js` contains plausible, benign-looking logger code. The real attack begins via a `postinstall` script in `package.json` that runs `node print.cjs`, which detaches a background process to fetch platform-specific binaries from a public Hugging Face repository (`Lordplay/system-releases`) and execute them silently.

The resulting implant is cross-platform (Windows, macOS, Linux) and offers:
- File read/write across the filesystem
- Keystroke logging and clipboard monitoring
- Secret-scanning for credentials
- Self-updating via HF `version.txt` polling (no signature or checksum verification)
- Archival of stolen data to **private attacker-controlled Hugging Face datasets**

Earlier campaign versions used Hetzner-hosted C2 (IP `195.201.194.107:8010`, ThreatFox confirmed botnet); the shift to Hugging Face infrastructure represents a deliberate migration to a legitimate, rarely-blocked cloud service.

---

## Why This Case Matters

This is the first widely-reported supply chain campaign to use a legitimate AI platform as both **CDN and exfiltration backend**. The choice is deliberate:

- **Hugging Face traffic looks normal** from dev endpoints running ML workloads
- **HF dataset uploads resemble legitimate model training pipelines**
- **No custom C2 infrastructure to seize** — takedown requires coordinating with Hugging Face for each attacker account
- **Dev tooling allow-lists work against defenders** — ML engineers legitimately need HF access
- **Attacker signatures are minimal** because the heavy lifting is done by HF's own infrastructure

For defenders, this forces a rethink: "connection to huggingface.co" used to be a trust signal. Now it's ambiguous.

---

## Attack Chain

```
1. Developer runs: npm install js-logger-pack
          │
          ▼
2. package.json postinstall hook fires → node print.cjs
          │
          ▼
3. print.cjs detaches background process (parent npm returns normally)
          │
          ▼
4. Background process detects OS (Windows / macOS / Linux)
          │
          ▼
5. Downloads MicrosoftSystem64 binary from huggingface.co/Lordplay/system-releases
          │
          ▼
6. Executes the binary silently
          │
          ▼
7. Implant establishes persistence (autostart on all three OSes)
          │
          ▼
8. Implant polls HF version.txt periodically for self-update
          │
          ▼
9. On operator command: archives target files/directories as gzip
          │
          ▼
10. Creates or reuses private HF dataset (derived from agentId + path)
          │
          ▼
11. Uploads stolen archive to attacker's HF account
```

---

## MITRE ATT&CK Mapping

| Tactic | Technique | ID |
|---|---|---|
| Initial Access | Supply Chain Compromise: Compromise Software Dependencies | [T1195.002](https://attack.mitre.org/techniques/T1195/002/) |
| Execution | Command and Scripting Interpreter: JavaScript | [T1059.007](https://attack.mitre.org/techniques/T1059/007/) |
| Execution | User Execution: Malicious File | [T1204.002](https://attack.mitre.org/techniques/T1204/002/) |
| Persistence | Boot or Logon Autostart Execution | [T1547](https://attack.mitre.org/techniques/T1547/) |
| Defense Evasion | Masquerading: Match Legitimate Name (`MicrosoftSystem64`) | [T1036.005](https://attack.mitre.org/techniques/T1036/005/) |
| Defense Evasion | Hide Artifacts (plausible benign `dist/index.js`) | [T1564](https://attack.mitre.org/techniques/T1564/) |
| Credential Access | Credentials from Password Stores: Web Browsers | [T1555.003](https://attack.mitre.org/techniques/T1555/003/) |
| Credential Access | Unsecured Credentials: Credentials In Files | [T1552.001](https://attack.mitre.org/techniques/T1552/001/) |
| Collection | Input Capture: Keylogging | [T1056.001](https://attack.mitre.org/techniques/T1056/001/) |
| Collection | Archive Collected Data: Archive via Utility (gzip) | [T1560.001](https://attack.mitre.org/techniques/T1560/001/) |
| Command and Control | Web Service: Bidirectional Communication (Hugging Face) | [T1102.002](https://attack.mitre.org/techniques/T1102/002/) |
| Command and Control | Ingress Tool Transfer (binary from HF repo) | [T1105](https://attack.mitre.org/techniques/T1105/) |
| Exfiltration | Exfiltration Over Web Service | [T1567](https://attack.mitre.org/techniques/T1567/) |

---

## Indicators of Compromise

### File Hashes (SHA256)
| Hash | Context |
|---|---|
| `8b1feda2fc7b4fb7db54a00be44b2a16ccc04ba8e487e46662dd693004b0155e` | Sample associated with js-logger-pack (source: ThreatFox IOC #1792046) |

### Network IOCs
| Indicator | Type | Context |
|---|---|---|
| `195.201.194.107:8010` | IP:port (TCP) | Botnet C2, earlier campaign phase — Hetzner-hosted, ThreatFox 100% confidence |
| `huggingface.co/Lordplay/system-releases` | URL | Malicious HF repository hosting `MicrosoftSystem64` binaries and `version.txt` |
| `huggingface.co/Lordplay` | URL | Attacker-controlled HF account |

### Package IOCs
| Indicator | Context |
|---|---|
| `js-logger-pack` (all versions) | Malicious npm package |
| `js-logger-pack@1.1.27` | Specifically confirmed — drops `MicrosoftSystem64` |

### Binary Names
| Indicator | Context |
|---|---|
| `MicrosoftSystem64` | Dropped binary (Windows, macOS, Linux variants) |
| `print.cjs` | First-stage dropper run from `postinstall` |

### Threat Actor Aliases / Personas
| Alias | Notes |
|---|---|
| `Lordplay` | Hugging Face account hosting malicious repo |
| `whisdev`, `snipmaxi`, `jrodacooker.dev` | Campaign-adjacent personas (per JFrog) |
| `joshstevens19` | Likely spoofed git authorship to misattribute |
| `bink` | Alias overlap — tracking only, not attribution |

---

## VirusTotal Enrichment (2026-04-24)

Enrichment performed via the Wazuh lab's VirusTotal integration (configured 2026-04-23).

| Observable | VT Verdict | Notes |
|---|---|---|
| `huggingface.co` | Legitimate (no malicious vendor detections) | Domain is benign; misuse is at the repository level |
| `js-logger-pack` (search) | Sample hash returned: `8b1feda2...` | Cross-referenced to ThreatFox IOC #1792046 (botnet C2, confidence 100%) |
| `huggingface.co/Lordplay/system-releases` (URL) | No VT match | HF repository URLs are not well-indexed by VT — **detection gap** |

### Analyst Note — Detection Gap

VirusTotal does not meaningfully scan Hugging Face repository URLs. Enterprise defenders relying on VT for URL reputation will not be warned about malicious HF repos. This is a real blind spot that JFrog's research exposes — and one that will become more important as more attackers abuse AI platforms for distribution. Layering additional enrichment sources (abuse.ch ThreatFox, MalwareBazaar, sandbox detonation via ANY.RUN or Hybrid Analysis) is recommended to close this gap.

---

## Detection Guidance

Behavioral detection is more durable than signature-based detection for this class of attack, because the C2 and exfil channels live inside legitimate cloud services.

### 1. Block `npm install` postinstall scripts in sensitive environments
```
npm config set ignore-scripts true
```
This is the single most effective preventive control for this class of attack. Postinstall hooks are the primary execution trigger.

### 2. Endpoint monitoring for postinstall-launched processes
Sysmon EID 1 where `ParentImage` matches `node.exe` or `npm.exe` and `Image` is a detached child process (`print.cjs`, `.exe` binaries in temp paths). Flag node child processes that outlive their parent npm install.

### 3. Network rule — unexpected binary downloads from huggingface.co
- Sysmon EID 3 (Network Connection) to `huggingface.co` followed by Sysmon EID 11 (FileCreate) of an executable
- Correlation window: 5 seconds between HF connection and binary write
- Filter by endpoint role: developer workstations are expected to access HF; general office endpoints are not

### 4. Monitor for `MicrosoftSystem64` as a filename
- Sysmon EID 11 (FileCreate) `TargetFilename` contains `MicrosoftSystem64`
- System binaries do not have this name — legitimate Microsoft components use different conventions
- Very high-fidelity signal, low false-positive rate

### 5. Flag outbound traffic to known Hetzner C2 IPs from dev workstations
- `195.201.194.107:8010` — known C2 from earlier campaign phase
- Not all Hetzner traffic is malicious, but this specific IP + port is

### 6. Audit npm package installations organization-wide
- If any developer has installed `js-logger-pack` at any version, **assume the host is compromised**
- Rotate all credentials accessible from that host: AWS keys, npm tokens, SSH keys, database passwords, API keys, wallet seeds, browser-stored credentials

---

## Response Playbook (If You Installed This Package)

Per JFrog's guidance, if you or your CI/CD pipeline installed `js-logger-pack@1.1.27` (or any version that dropped `MicrosoftSystem64` binaries):

1. **Assume full compromise** of the machine or CI/CD runner
2. **Rotate all accessible secrets** — AWS, npm, SSH, DB, API keys, wallet seeds, browser credentials
3. **Audit registry and source-control logs** for unauthorized access or pushes
4. **Set `npm config set ignore-scripts true`** globally
5. **Remove `js-logger-pack`** and any packages that declared it as a transitive dependency
6. **Review `package.json` changes** in recent dependency updates, even patch releases
7. **Forensic imaging** of the affected host before cleanup if regulated data was present

---

## Analyst Note — Why This Case Is a Big Deal

Most supply chain attacks follow a familiar pattern: malicious package → custom C2 server → takedown by registry or host. Takedowns work because attackers depend on dedicated infrastructure that defenders can seize.

js-logger-pack represents a different approach. By using Hugging Face as both CDN and exfiltration backend:

- **There's no custom C2 to seize** — HF hosts it all
- **Takedown requires coordination with a third party** — slower, imperfect, attacker can recreate accounts
- **Dev tooling allow-lists work against defenders** — ML engineers legitimately need HF access
- **Exfiltrated data lives in "legitimate" private datasets** — disguised as benign model training data

The pattern is likely to repeat across other AI/ML platforms (Kaggle, Colab, Replicate, and others) as attackers generalize the approach. Defenders need to start treating AI platform traffic with the same scrutiny as any other C2 channel.

**Patient zero:** JFrog researchers found this by monitoring npm registry telemetry and identifying a package with anomalous install patterns and a postinstall script fetching external binaries. That's the kind of proactive registry monitoring that catches supply chain attacks before they spread widely. No individual developer running `npm install` would have caught this on their own.

---

## Case Status

Intake complete. No evidence of `js-logger-pack` installation in this lab environment (no npm-based development occurs here). Case retained as threat intel reference for future detection rule development.

### Follow-up Tasks

- [ ] Review any Hugging Face connections from lab machines in the last 30 days (unlikely but worth checking)
- [ ] Add `MicrosoftSystem64` filename to Wazuh FIM watch rules
- [ ] Add hash `8b1feda2fc7b4fb7db54a00be44b2a16ccc04ba8e487e46662dd693004b0155e` to VirusTotal-watched observable list
- [ ] Evaluate ThreatFox integration for Wazuh to catch future supply chain IOCs that VT misses
- [ ] Monitor for additional IOCs as JFrog and others update the public record

---

## References

- JFrog Security Research — https://research.jfrog.com/post/hugging-face-exfil/
- SafeDep (earlier phase) — https://safedep.io/malicious-js-logger-pack-npm-stealer/
- ThreatFox IOC #1792046 — https://threatfox.abuse.ch/ioc/1792046/
- Cybersecurity News — https://cybersecuritynews.com/malicious-npm-package-turns-hugging-face/
- MITRE ATT&CK — https://attack.mitre.org/
```
