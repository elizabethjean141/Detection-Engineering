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
*(Writeups coming soon)*

---
## Notes
This lab documents both successful detections and failures. 
Blind spots and pipeline gaps are included intentionally-
Understanding why a detection rule fails is as valuable as 
confirming it works. 

