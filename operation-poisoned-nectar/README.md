Case #3 — Operation Poisoned Nectar: Vishing-Based Initial Access
Case Overview
FieldValueTitleOperation Poisoned Nectar: Vishing-Based Initial AccessSeverityMEDIUMTLPAMBERPAPAMBERAssignee<analyst@soc.local>Incident Start Time2026-04-14 08:49Case Creation Time2026-04-14 08:55Case Close Time2026-04-14 09:23StatusResolved

Description
The victim was presented with a fraudulent popup warning indicating system infection. The message included a phone number prompting immediate contact for technical support. Upon interaction, the attacker leveraged social engineering tactics to establish trust and guide the victim toward actions that could result in system compromise.
Analysis Focus

Identification of social engineering indicators (urgency, fear-based messaging)
Validation of malicious infrastructure (URL, phone number)
Correlation with potential follow-on endpoint activity


MITRE ATT&CK TTPs
TacticTechniquePattern IDOccur DateInitial AccessPhishing — VishingT15662026-04-14 08:49ExecutionUser ExecutionT12042026-04-14 08:49Defense EvasionMasqueradingT10362026-04-14 08:49

Observables
Data TypeValueIOCTLP————

No technical observables captured at time of case creation. Social engineering vector — infrastructure validation pending.


Investigation Tasks
TaskStatusNotesValidate Social Engineering Indicators✅ CompletedUrgency and fear-based messaging confirmed as scareware tacticsMap Behavior to MITRE ATT&CK✅ CompletedT1566, T1204, T1036 mappedDocument Findings and Final Assessment✅ CompletedSee assessment below

Assessment
Initial access vector confirmed as social engineering via scareware popup.
Further investigation required to determine extent of system compromise and any follow-on activity.
Beekeeper Insight

The threat does not begin with malware — it begins with manipulation. The attacker gains access not by breaking the system, but by convincing the user to open the door.

This suggests potential progression from initial social engineering access to post-compromise reconnaissance. This activity may represent post-compromise reconnaissance following user-initiated access documented in this case.

Analyst Notes
Vishing (voice phishing) combined with scareware popups is a well-documented initial access technique used by tech support scam operations and ransomware affiliate groups. The attack chain typically follows this pattern:



  
