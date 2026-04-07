# Detection Engineering Home Lab

MITRE ATT&CK mapped detection engineering project built on a Windows/Ubuntu VM stack. Simulations run via Atomic Red Team, telemetry captured through Sysmon and analyzed in Wazuh.

## Lab Stack
### Current

-**SIEM:** Wazuh (Ubuntu VM)
-**NDR:** Suricata (Ubuntu VM)
-**Endpoint:** Windows 11 VM with Sysmon + Wazuh Agent 
-**Simulation:** Atomic Red Team
-**Framework:** MITRE ATT&CK
-**Attack Node:** Kali Linux

### Previous
-**SIEM:** Splunk Enterprise with Universal Forwarder
-Detections built in this environment are documented below.

## Detection Coverage

| Technique | Name | Status |
|-----------|------|--------|
| T1218.010 | Regsvr32 / Squiblydoo | ✅ Detected |
| T1055 | Process Injection | ⚠️ Partial - blind spot documented |
| T1003.001 | LSASS Dump | 🛡️ Blocked by RunAsPPL |
| T1003.002 | SAM Hive Dump | ✅ Detected |
| T1112 | LSA Registry Modification | ✅ Detected |
| T1059.001 | PowerShell Execution | ✅ Detected |
| T1547.001 | Registry Run Keys | ✅ Detected |
---
## Detections
## T1016 – System Network Configuration Discovery (ipconfig.exe)

### Summary
Observed execution of ipconfig.exe to retrieve network configuration details. While benign in isolation, this behavior is commonly used during early-stage discovery.

---

### Telemetry Source
- Sysmon Event ID 1 (Process Creation)
- Sysmon Event ID 5 (Process Termination)

---

### Detection Logic
- Identify execution of ipconfig.exe
- Correlate with parent process
- Flag unusual parent-child relationships

---

### Example Query (Wazuh / Sysmon-style logic)
Image="*ipconfig.exe*"

---

### Behavioral Context
Normal:
- Parent: powershell.exe, cmd.exe
- Single execution

Suspicious:
- Parent: winword.exe, excel.exe, unknown binary
- Repeated execution (looping behavior)
- Short execution bursts across multiple hosts

---

### Why It Matters
Indicates potential **discovery phase activity** post-compromise.  
When combined with other signals (whoami, systeminfo), can indicate attacker reconnaissance.

---

### Detection Gaps
- High false positives if not correlated with parent process
- Requires baseline of normal administrative behavior

---

### MITRE ATT&CK
T1016 – System Network Configuration Discovery

---
## Notes
This lab documents both successful detections and failures. 
Blind spots and pipeline gaps are included intentionally-
Understanding why a detection rule fails is as valuable as 
confirming it works. 

