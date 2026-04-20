Detection Engineering Home Lab

MITRE ATT&CK mapped detection engineering project built on a Windows/Ubuntu bare-metal and VM stack. Simulations run via Atomic Red Team, telemetry captured through Sysmon and analyzed in Wazuh with automated alert forwarding to TheHive.


Lab Stack
Current
ComponentTechnologySIEMWazuh (Ubuntu bare metal)Case ManagementTheHive 5 (Ubuntu bare metal)NDRSuricata (Ubuntu)EndpointWindows 11 VM — Sysmon + Wazuh AgentSimulationAtomic Red TeamFrameworkMITRE ATT&CKAttack NodeKali Linux
Previous
ComponentTechnologySIEMSplunk Enterprise with Universal Forwarder

Detections built in the Splunk environment are documented below. The lab was migrated to Wazuh to expand detection coverage and integrate with TheHive for case management.


Detection Coverage
TechniqueNameStatusT1218.010Regsvr32 / Squiblydoo✅ DetectedT1055Process Injection⚠️ Partial — blind spot documentedT1003.001LSASS Dump🛡️ Blocked by RunAsPPLT1003.002SAM Hive Dump✅ DetectedT1112LSA Registry Modification✅ DetectedT1059.001PowerShell Execution✅ DetectedT1059.001Obfuscated PowerShell (-enc)✅ DetectedT1547.001Registry Run Keys✅ DetectedT1136.001Local Account Creation (Event ID 4720)✅ DetectedT1016System Network Configuration Discovery✅ DetectedT1562.001Impair Defenses — Agent Disconnect✅ Detected

Detections

T1136.001 — Local Account Creation (Event ID 4720)
Summary
Detected successful local account creation via Windows Security Event ID 4720. Custom Wazuh rule written to correctly map this event to T1136.001, overriding the default incorrect mapping to T1484.
Telemetry Source

Windows Security Auditing (Event ID 4720)
Sysmon Event ID 1 (Process Creation — net.exe)

Custom Wazuh Rule

