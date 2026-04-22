# Lotus Wiper — Proposed Detection Rules

Rules are proposed for the Threat Huntress Lab Wazuh deployment. Rule IDs follow the lab's existing scheme: `100200-100299` for Windows detections.

## Rule Candidates

### 1. `C:\lotus\` Staging Directory Access (T1059.003 + T1105)

- **Proposed ID:** 100230
- **Wazuh parent:** Sysmon EID 11 (FileCreate)
- **Trigger:** `TargetFilename` contains `\lotus\`
- **Severity:** Level 13
- **Rationale:** Any file write into a `\lotus\` path on a non-admin workstation is suspicious

### 2. System Binary in Unusual Location (T1036.005)

- **Proposed ID:** 100231
- **Wazuh parent:** Sysmon EID 11 (FileCreate)
- **Trigger:** File creation matching `cmd.exe`, `powershell.exe`, `wscript.exe`, etc. in any path that is not `C:\Windows\System32\` or `C:\Windows\SysWOW64\`
- **Severity:** Level 13
- **Rationale:** Lotus copies `cmd.exe` to `C:\lotus\`. This pattern is broader than Lotus and catches many threat actor techniques.

### 3. UI0Detect Service Stop (T1562.001)

- **Proposed ID:** 100232
- **Wazuh parent:** Windows Security EID 7036
- **Trigger:** Service name = `Interactive Services Detection` (UI0Detect) AND state = Stopped
- **Severity:** Level 12
- **Rationale:** Narrow, specific, low false-positive. Legitimate stops are very rare.

### 4. LOLBAS Correlation — diskpart / fsutil / robocopy (T1561 + T1490)

- **Proposed ID:** 100233 (requires correlation rule logic)
- **Wazuh parent:** Sysmon EID 1 (ProcessCreate)
- **Trigger:** 3+ distinct executions of (`diskpart.exe` OR `fsutil.exe` OR `robocopy.exe`) within 5-minute window on a single host
- **Severity:** Level 14
- **Rationale:** Individually these are legit admin tools. Clustered execution is the signal.

### 5. Volume Shadow Copy Deletion (T1490)

- **Proposed ID:** 100234
- **Wazuh parent:** Sysmon EID 1 (ProcessCreate)
- **Trigger:** `CommandLine` matches `vssadmin delete shadows` OR `wbadmin delete catalog`
- **Severity:** Level 14
- **Rationale:** Classic ransomware / wiper precursor. Near-zero false positives on workstations.

### 6. Cached Logon Policy Modification (T1556)

- **Proposed ID:** 100235
- **Wazuh parent:** Sysmon EID 13 (RegistryValueSet)
- **Trigger:** Target object matches `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\CachedLogonsCount`
- **Severity:** Level 11
- **Rationale:** Rare change on workstations; should be baseline-suppressed for GPO-managed domain policy paths

### 7. Mass Session Logoff (T1531)

- **Proposed ID:** 100236 (correlation rule)
- **Wazuh parent:** Windows Security EID 4634 OR EID 4647
- **Trigger:** 10+ logoff events on a single host within 60 seconds
- **Severity:** Level 13
- **Rationale:** Logoffs happen constantly, but 10+ in 60s indicates something forced it.

## Implementation Priority

| # | Rule | Priority | Why |
|---|---|---|---|
| 1 | UI0Detect stop (100232) | **High** | Smallest, narrowest, fastest win |
| 2 | VSS deletion (100234) | **High** | Catches far more than just Lotus — all ransomware/wipers |
| 3 | System binary in unusual location (100231) | **High** | Broadly applicable across many campaigns |
| 4 | `\lotus\` staging (100230) | Medium | Narrow but useful while campaign is active |
| 5 | LOLBAS correlation (100233) | Medium | Requires correlation logic — more complex |
| 6 | Mass logoff (100236) | Medium | Requires correlation logic |
| 7 | Cached logon policy change (100235) | Low | Rare legitimate changes, but still some noise |

## Known Blind Spots (Documented Gaps)

These techniques are **not** practical to detect from the Wazuh + Sysmon stack this lab currently runs:

- **T1561.002 — Physical sector wipe via IOCTL calls.** Requires kernel-level telemetry (EDR driver, not standard Sysmon).
- **T1070.001 — USN journal clearing.** Same limitation; Sysmon has no visibility into NTFS metadata operations.
- **T1498 — Network interface deactivation.** No baseline Wazuh rule; would require custom netsh/WMI monitoring.

These blind spots are **intentional** — documented as gaps in coverage rather than hidden. A future upgrade to a commercial EDR or kernel-level monitoring would close them.
