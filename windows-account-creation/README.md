Detection Case: Windows Account Creation (Event ID 4720)
T1136.001 — Local Account Creation via Native Windows Utilities

Summary
Simulated local account creation using native Windows utilities and validated detection across endpoint telemetry, Windows Security logs, and Wazuh SIEM ingestion.
This case demonstrates the difference between:
LayerWhat It ShowsCommand execution visibility (Sysmon)Intent — the command was runConfirmed system state change (Security Event Logs)Reality — the account was createdSIEM-level detection and enrichment (Wazuh)Visibility and context — parsed, alerted, mapped

Objective
Validate that a locally created user account:

Generates a Windows Security Event (4720)
Is ingested and parsed by Wazuh
Can be mapped to MITRE ATT&CK for detection engineering


Lab Setup
ComponentDetailsEndpointWindows 11 VM (Sysmon enabled)SIEMWazuh Manager (Ubuntu bare metal)AgentWazuh agent installed on Windows VMLog SourcesMicrosoft-Windows-Sysmon/Operational, Security

Attack Simulation
Executed using native Windows command:



