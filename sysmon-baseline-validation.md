Case #1 — Sysmon Process Creation: Baseline Validation

🔒 TLP: AMBER — For internal use and trusted partners only.


Case Overview
FieldValueTitleSysmon Process Creation — Baseline ValidationSeverityMEDIUMTLPAMBERPAPAMBERAssignee<analyst@soc.local>Incident Start Time2026-04-14 00:25Case Creation Time2026-04-14 00:30Case Close Time—StatusIn Progress

Description
This case was created to validate TheHive deployment and confirm end-to-end incident response workflow functionality. The objective is to simulate a basic detection scenario using Sysmon Event ID 1 (process creation), document findings, and validate case lifecycle operations including task management, observables, and analyst notes.
This serves as a baseline case for future detection engineering and threat hunting investigations.

Summary
Baseline Sysmon Event ID 1 activity was analyzed to validate expected process execution behavior.

Observed commands (ipconfig.exe, whoami.exe, systeminfo.exe) aligned with standard administrative usage
Parent-child relationships reviewed and confirmed to originate from legitimate shells (cmd.exe, powershell.exe)
No anomalous execution chains or suspicious parent processes identified
Relevant MITRE ATT&CK Discovery techniques mapped (T1016, T1033, T1082)
No indicators of compromise detected


This baseline will be used as a reference point for detecting deviations in future process execution investigations.


Observables
1. ipconfig.exe
FieldValueData TypeotherDataipconfig.exeIOCSightedTLPAMBERCreated2026-04-14 00:33
Detail: Initial validation case created following successful deployment of TheHive. Focus is on establishing a repeatable investigation workflow and validating integration with endpoint telemetry sources (Sysmon). No confirmed malicious activity at this stage. This observable is used for baseline and workflow verification.

2. ipconfig /all
FieldValueData TypeotherDataipconfig /allIOCSightedTLPAMBERCreated2026-04-14 00:37

MITRE ATT&CK TTPs
TacticTechniqueNameOccur DateDiscoveryT1082System Information Discovery2026-04-14 01:31DiscoveryT1016.001Internet Connection Discovery2026-04-14 01:31DiscoveryT1033System Owner/User Discovery2026-04-14 01:20

Investigation Tasks
Task 1: Validate Parent-Child Process Relationship
FieldValueStatusIn ProgressStart Date2026-04-14 01:04
Analyst Notes — 2026-04-14 01:04:
Reviewed expected parent processes for ipconfig.exe execution.
Legitimate parent processes include:

cmd.exe
powershell.exe

Execution from any non-standard parent process (e.g., winword.exe or unknown binaries) would be considered suspicious and require further investigation.
Finding: No anomalous parent-child relationships identified during this validation step.

Task 2: Analyze Process Execution Activity
FieldValueStatusIn ProgressStart Date2026-04-14 01:13
Analyst Notes — 2026-04-14 01:13:
Reviewed Sysmon Event ID 1 logs for baseline execution patterns. Observed standard administrative commands:

ipconfig.exe
whoami.exe
systeminfo.exe

Finding: No anomalous execution patterns identified.

Baseline Process Execution Reference
This case establishes the following as known-good behavior for this environment:
BinaryExpected ParentExpected UserAssessmentipconfig.execmd.exe, powershell.exeAnalyst✅ Normalwhoami.execmd.exe, powershell.exeAnalyst✅ Normalsysteminfo.execmd.exe, powershell.exeAnalyst✅ Normal
Deviation from this baseline should trigger investigation:
DeviationRisk LevelNon-standard parent process🔴 HighExecution outside business hours🟡 MediumRapid sequential execution of all three commands🟡 MediumExecution from Temp or AppData path🔴 HighUnknown user context🔴 High

Validation Checklist
Validation ItemStatusTheHive deployment confirmed✅End-to-end workflow validated✅Sysmon Event ID 1 telemetry confirmed✅Case lifecycle operations tested✅Task management functional✅Observable documentation functional✅MITRE ATT&CK mapping functional✅Analyst notes and logs functional✅Integration with endpoint telemetry confirmed✅

Significance of This Case
Case #1 is the foundation of the detection engineering pipeline. Before any detection rule can be trusted, the baseline must be established and validated.

You cannot detect deviation without first defining normal.

This case documents that normal. Every subsequent detection case in this lab is measured against the baseline established here.

Related Cases

Case #2 — Operation Rogue Worker — ipconfig.exe observed in post-compromise context
Investigation — Suspicious Process Execution ipconfig.exe — deeper analysis of same binary in different context



🔒 TLP: AMBER — This document is classified TLP:AMBER. Do not distribute beyond trusted partners.


Detection Engineering Lab — github.com/elizabethjean141/Detection-Engineering

 

