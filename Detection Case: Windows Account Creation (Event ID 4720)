 Detection Case: Windows Account Creation (Event ID 4720)
Summary

Simulated local account creation using native Windows utilities and validated detection across endpoint telemetry, Windows Security logs, and Wazuh SIEM ingestion.

This case demonstrates the difference between:

Command execution visibility (Sysmon)
Confirmed system state change (Security Event Logs)
SIEM-level detection and enrichment (Wazuh)
 Objective

Validate that a locally created user account:

Generates a Windows Security Event (4720)
Is ingested and parsed by Wazuh
Can be mapped to MITRE ATT&CK for detection engineering
 Lab Setup
Endpoint: Windows 11 VM (Sysmon enabled)
SIEM: Wazuh Manager (Ubuntu)
Agent: Wazuh agent installed on Windows
Log Sources:
Microsoft-Windows-Sysmon/Operational
Security
Attack Simulation

Executed using native Windows command:

net user final4720test Pass123! /add

 Result:

The command completed successfully.
Step 1: Verify System-Level Event (Ground Truth)

Queried Windows Security Log:

Get-EventLog -LogName Security -InstanceId 4720 -Newest 5

Result:

Event ID 4720
Message: “A user account was created”

Evidence:

Step 2: Confirm SIEM Ingestion (Wazuh)

Wazuh successfully ingested and parsed the event:

"targetUserName": "final4720test",
"subjectUserName": "Analyst"

Evidence:

Detection Analysis
What was detected:
Successful account creation
Actor (user: Analyst)
Target account (final4720test)
Observed Limitation:

Wazuh mapped the event to:

T1484 – Domain Policy Modification

This is not the most accurate mapping.

Correct MITRE Mapping:
T1136 – Create Account
Sub-technique: T1136.001 – Local Account
Key Insight

Detection is not just about seeing a command — it’s about confirming the outcome.

Sysmon shows intent (command execution)
Security logs confirm reality (account created)
Wazuh provides visibility and context

Failed commands generate telemetry — but only successful execution generates true security events.

Detection Pipeline Validation
Layer	Status
Sysmon (Process Execution)	
Windows Security Log (4720)	
Wazuh Ingestion & Parsing	
MITRE Mapping Accuracy	 Needs tuning
Next Steps
Tune Wazuh rules to correctly map:
4720  T1136.001
Correlate with:
Parent process (Sysmon Event ID 1)
Logon session context
Build detection for:
Suspicious account naming patterns
Rapid account creation bursts
Why This Matters

Account creation is a common persistence technique.

Without:

Proper logging
Accurate parsing
Correct MITRE mapping

Detection either fails silently or misleads analysts.

Closing Thought

This case reinforces a core detection principle:

You can’t trust detection unless you verify the data layer first.
