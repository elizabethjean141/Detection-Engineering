Operation Ron's Coffee
Illicit Infrastructure Hosted Behind Legitimate Business

🔒 TLP: AMBER — For internal use and trusted partners only.


Case Overview
FieldValueTitleOperation Ron's Coffee: Illicit Infrastructure Hosted Behind Legitimate BusinessSeverityMEDIUMTLPAMBERPAPAMBERAssignee<analyst@soc.local>StatusInvestigation In ProgressConfidence LevelMedium

Overview
This case models an investigation where a seemingly legitimate business is used to host hidden malicious or unauthorized infrastructure.
The objective is to detect covert misuse of systems that appear normal on the surface but exhibit anomalous backend behavior.

Scenario
A local coffee shop provides public Wi-Fi and normal business services. However, subtle anomalies trigger investigation:

Unusual network traffic patterns
Persistent uptime inconsistent with business operations
Suspicion of hidden services hosted internally

Further analysis suggests the system is being used to host illicit or unauthorized services behind legitimate infrastructure.

Investigation Focus

Identification of abnormal network behavior
Detection of hidden or unauthorized services
Correlation between legitimate front-end and suspicious back-end activity
Validation of persistence mechanisms


Telemetry Analysis
Data Sources
SourcePurposeSysmonProcess creation, network connections, file activityNetwork telemetryTraffic volume, timing, destination analysisFirewall / Router logsPort exposure, inbound/outbound rule violationsSuricata / ZeekProtocol anomaly detection, IDS alerts
Observed Indicators
IndicatorSignificanceHigh outbound traffic outside normal business hoursSuggests automated or covert activityConnections to unknown external IPsPotential C2 or exfiltration destinationsLong-running services inconsistent with business purposePersistence or unauthorized hostingOpen ports not aligned with café operationsHidden service exposure

Contextual Analysis
Individually, these signals may appear benign.
Together, they suggest intentional misuse of legitimate infrastructure.

Key Insight

The system was not necessarily exploited.
It may have been repurposed.

This may indicate:

Rogue admin behavior — insider with elevated access
Insider misuse — employee leveraging business infrastructure for personal gain
Covert criminal use — external actor using legitimate business as cover for illicit operations


MITRE ATT&CK TTPs
TacticTechniquePattern IDCommand and ControlProxyT1090Command and ControlProtocol TunnelingT1572Command and ControlApplication Layer ProtocolT1071DiscoveryNetwork Service DiscoveryT1046ExfiltrationExfiltration Over Web ServicesT1567

Assessment
AreaStatusInitial AccessUnknownPersistenceSuspectedDiscoveryObservedCommand and ControlSuspectedExfiltrationSuspected
Confidence Level: Medium

Rationale

Sustained abnormal network behavior outside business hours
Infrastructure use inconsistent with stated business purpose
Indicators of concealed or unauthorized communication channels
Pattern consistent with proxy or tunneling activity (T1090, T1572)


Detection Chain



