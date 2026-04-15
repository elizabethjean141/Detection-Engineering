
Case #1 - Sysmon Process Creation- Baseline Validation
Case overview
Information

Title: Sysmon Process Creation- Baseline Validation
Severity: MEDIUM
TLP: AMBER
PAP: AMBER
Assignee: elizabeth@thehive.local
Incident start time: 2026-04-14 00:25
Case creation time: 2026-04-14 00:30
Case close time:
Description

This case was created to validate TheHive deployment and confirm end-to-end incident response workflow functionality.
The objective is to simulate a basic detection scenario using Sysmon Event ID 1 (process creation), document findings, and validate case lifcycle operations including task management, observables, and analyst notes.
This serves as a baseline case for future detection engineering and threat hunting investigations.
Summary: Baseline Sysmon Event ID 1 activity was analyzed to validate expected process execution behavior. Observed commands (ipconfig.exe, whoami.exe, systeminfo.exe) aligned with standard administrative usage. Parent-child relationships were reviewed and confirmed to originate from legitimate shells (cmd.exe, powershell.exe). No anomalous execution chains or suspicious parent processes were identified. Relevant MITRE ATT&CK Discovery techniques were mapped (T1016, T1033, T1082). No indicators of compromise were detected.
This baseline will be used as a reference point for detecting deviations in future process execution investigations.
Additional information
Summary
Observables

    Data type
    other
    Data
    ipconfig/all
    Ioc
    Sighted
    Message
    Created at
    2026-04-14 00:37
    Tlp
    AMBER

    Data type
    other
    Data
    ipconfig.exe
    Ioc
    Sighted
    Message
    Initial validation case created following successful deployment of TheHive. Focus is on establishing a repeatable investigation workflow and validating integration with endpoint telemetry sources (Sysmon). No confirmed malicious activity at this stage. This case is used for baseline and workflow verification.
    Created at
    2026-04-14 00:33
    Tlp
    AMBER

TTPs
Technique	Pattern id	Occur date
Discovery	T1082 System Information Discovery	2026-04-14 01:31
Discovery	T1016.001 Internet Connection Discovery	2026-04-14 01:31
Discovery	T1033 System Owner/User Discovery	2026-04-14 01:20
Tasks

    Title
        Validate Parent-Child Process Relationship
    Group
        default
    Assignee
        elizabeth@thehive.local
    Status
        InProgress
    Start date
        2026-04-14 01:04
    Due date

    Logs
        2026-04-14 01:04
        :elizabeth@thehive.local

        Reviewed expected parent processes for ipconfig.exe execution. Legitimate parent processes include:
        -cmd.exe
        -powershell.exe
        Execution from any non-standard parent process (e.g., winword.exe or unknown binaries) would be considered suspicious and require further investigation.
        No anomalous parent-child relationships identified during this validation step.

    Title
        Analyze Process Execution Activity
    Group
        default
    Assignee
        elizabeth@thehive.local
    Status
        InProgress
    Start date
        2026-04-14 01:13
    Due date

    Logs
        2026-04-14 01:13
        :elizabeth@thehive.local

        Reviewed Sysmon Event ID 1 logs for baseline execution patterns. Observed standard administrative commands (ipconfig.exe, whoami.exe, systeminfo.exe). No anomalous execution patterns identified.

This document is classified TLP:AMBER
