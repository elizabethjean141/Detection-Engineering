
Case #3 - Operation Poisoned Nectar: Vishing-Based initial Access
Case overview
Information

Title: Operation Poisoned Nectar: Vishing-Based initial Access
Severity: MEDIUM
TLP: AMBER
PAP: AMBER
Assignee: elizabeth@thehive.local
Incident start time: 2026-04-14 08:49
Case creation time: 2026-04-14 08:55
Case close time:
Description

The victim was presented with a fraudulent popup warning indicating system infection. The message included a phone number prompting immediate contact for technical support.
Upon interaction, the attacker leveraged social engineering tactics to establish trust and guide the victim toward actions that could result in system compromise.
Analysis Focus:
- Identification of social engineering indicators (urgency, fear-based messaging).
- Validation of malicious infrastructure (URL, phone number).
- Correlation with potential follow-on endpoint activity.
MITRE ATT&CK Mapping:
- T1566: Phishing (Vishing)
- T1204: User Execution
- T1036: Masquerading
Assessment:
Initial access vector confirmed as social engineering via scareware popup. Further investigation required to determine extent of system compromise and any follow-on activity.
Beekeeper Insight:
The threat does not begin with malware- it begins with manipulation. The attacker gains access not by breaking the system, but by convincing the user to open the door.
This suggests potential progression from initial social engineering access to post-compromise reconnaissance.
This activity may represent post-compromise reconnaissance following user-initiated access documented in Case #3 (Vishing-Based Initial Access).
Additional information
Summary
Observables

TTPs
Technique	Pattern id	Occur date
Tasks

    Title
        Validate social engineering indicators
    Group
        default
    Assignee
    Status
        Completed
    Start date
        2026-04-14 09:23
    Due date

    Title
        Document findings and final assessment
    Group
        default
    Assignee
    Status
        Completed
    Start date
        2026-04-14 09:23
    Due date

    Title
        Map behavior to MITRE ATT&CK
    Group
        default
    Assignee
    Status
        Completed
    Start date
        2026-04-14 09:23
    Due date

This document is classified TLP:AMBER
