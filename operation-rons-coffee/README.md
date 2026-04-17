# peration Ron's Coffee  
## Illicit Infrastructure Hosted Behind Legitimate Business

### Overview
This case models an investigation where a seemingly legitimate business is used to host hidden malicious or unauthorized infrastructure.

The objective is to detect covert misuse of systems that appear normal on the surface but exhibit anomalous backend behavior.

---

## Scenario
A local coffee shop provides public Wi-Fi and normal business services.

However, subtle anomalies trigger investigation:
- Unusual network traffic patterns
- Persistent uptime inconsistent with business operations
- Suspicion of hidden services hosted internally

Further analysis suggests the system is being used to host illicit or unauthorized services behind legitimate infrastructure.

---

## 🔍 Investigation Focus
- Identification of abnormal network behavior
- Detection of hidden or unauthorized services
- Correlation between legitimate front-end and suspicious back-end activity
- Validation of persistence mechanisms

---

## Telemetry Analysis

### Data Sources
- Sysmon
- Network telemetry
- Firewall or router logs
- Suricata or Zeek if available

### Observed Indicators
- High outbound traffic outside normal business hours
- Connections to unknown external IPs
- Long-running services inconsistent with business purpose
- Potential open ports or hidden services not aligned with café operations

---

## Contextual Analysis
Individually, these signals may appear benign.

Together, they suggest intentional misuse of legitimate infrastructure.

---

## Key Insight
> The system was not necessarily exploited.  
> It may have been repurposed.

This may indicate:
- Rogue admin behavior
- Insider misuse
- Covert criminal use of legitimate assets

---

## MITRE ATT&CK Mapping
- **T1090** – Proxy
- **T1572** – Protocol Tunneling
- **T1046** – Network Service Discovery
- **T1071** – Application Layer Protocol
- **T1567** – Exfiltration Over Web Services

---

## Assessment
- **Initial Access:** Unknown
- **Persistence:** Suspected
- **Discovery:** Observed
- **Command and Control:** Suspected

### Confidence Level: Medium

---

## Rationale
- Sustained abnormal network behavior
- Infrastructure use inconsistent with business purpose
- Indicators of concealed or unauthorized communication

---

## Recommended Next Steps
- Identify all running services and process owners
- Audit the host for unauthorized software
- Inspect outbound connections for suspicious destinations
- Review firewall exposure and listening ports
- Perform host forensic review

---

## Beekeeper Insight
> Not every threat breaks in.  
> Some threats are already inside — hidden in plain sight.

---

## Related Concept
This case is inspired by the coffee shop investigation scene in *Mr. Robot* and reframed as a detection engineering scenario focused on covert infrastructure misuse.

---


