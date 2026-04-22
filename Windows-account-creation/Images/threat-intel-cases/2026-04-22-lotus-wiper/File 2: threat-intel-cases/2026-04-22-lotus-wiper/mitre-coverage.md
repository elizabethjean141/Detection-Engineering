# Lotus Wiper — MITRE ATT&CK Coverage Analysis

## Techniques Observed in This Campaign

| Tactic | Technique | ID | Evidence |
|---|---|---|---|
| Initial Access / Persistence | Valid Accounts | [T1078](https://attack.mitre.org/techniques/T1078/) | Pre-existing domain compromise |
| Execution | Command and Scripting Interpreter: Windows Command Shell | [T1059.003](https://attack.mitre.org/techniques/T1059/003/) | Two batch scripts drive the attack |
| Defense Evasion | Impair Defenses: Disable or Modify Tools | [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | UI0Detect service stopped |
| Defense Evasion | Indicator Removal: Clear Windows Event Logs | [T1070.001](https://attack.mitre.org/techniques/T1070/001/) | USN journal entries cleared |
| Defense Evasion | Indicator Removal: File Deletion | [T1070.004](https://attack.mitre.org/techniques/T1070/004/) | Systematic file deletion across volumes |
| Discovery | Account Discovery: Local Account | [T1087.001](https://attack.mitre.org/techniques/T1087/001/) | Batch script enumerates local users |
| Credential Access | Modify Authentication Process | [T1556](https://attack.mitre.org/techniques/T1556/) | Cached logins disabled |
| Command and Control | Ingress Tool Transfer | [T1105](https://attack.mitre.org/techniques/T1105/) | Remote XML retrieved from NETLOGON share |
| Impact | Account Access Removal | [T1531](https://attack.mitre.org/techniques/T1531/) | Active sessions logged off |
| Impact | Network Denial of Service | [T1498](https://attack.mitre.org/techniques/T1498/) | Network interfaces deactivated |
| Impact | Data Destruction | [T1485](https://attack.mitre.org/techniques/T1485/) | Core wiper activity |
| Impact | Disk Wipe: Disk Content Wipe | [T1561.001](https://attack.mitre.org/techniques/T1561/001/) | `diskpart clean all` wipes logical drives |
| Impact | Disk Wipe: Disk Structure Wipe | [T1561.002](https://attack.mitre.org/techniques/T1561/002/) | Physical sector overwrite, USN clear |
| Impact | Inhibit System Recovery | [T1490](https://attack.mitre.org/techniques/T1490/) | Restore points deleted, disk filled |

## Current Lab Detection Coverage

Status of existing Wazuh rules (in `local_rules.xml`) against Lotus Wiper TTPs:

| Technique | Existing Rule | Status | Notes |
|---|---|---|---|
| T1059.001 | 100202 (PowerShell -enc) | ⚠️ Partial | Covers PowerShell, not cmd.exe. T1059.003 needs its own rule. |
| T1059.003 | None | ❌ Gap | Would detect batch execution patterns |
| T1562.001 | 100210 (agent disconnect) | ⚠️ Partial | Covers agent impairment; not UI0Detect specifically |
| T1070.001 | None | ❌ Gap | USN journal clearing has no existing detection |
| T1070.004 | None | ❌ Gap | Mass file deletion not currently monitored |
| T1078 | None | ❌ Gap | Valid account abuse is hard to signature — requires baseline |
| T1087.001 | None | ❌ Gap | Local account enumeration could be detected via EID 4798 |
| T1556 | None | ❌ Gap | Registry modification of cached-logon policy |
| T1105 | None | ❌ Gap | NETLOGON XML retrieval anomaly detection |
| T1485 | None | ❌ Gap | Data destruction signals |
| T1561.001 | None | ❌ Gap | `diskpart clean` invocation detection |
| T1561.002 | None | ❌ Gap | Physical sector wipe (requires kernel-level visibility) |
| T1490 | None | ❌ Gap | VSS deletion / wbadmin delete catalog |
| T1531 | None | ❌ Gap | Mass logoff pattern |
| T1498 | None | ❌ Gap | Network interface deactivation |

## Gap Summary

Out of 14 observed techniques, **0 have full existing coverage**, **2 have partial coverage**, and **12 are gaps**. This is expected for an entry-level lab — but worth documenting as intentional blind spots until rules are built. See [`detection-opportunities.md`](./detection-opportunities.md) for proposed new rules.
