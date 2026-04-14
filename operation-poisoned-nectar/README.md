📸 Supporting Evidence






🐝 Operation Poisoned Nectar: Vishing-Based Initial Access

📌 Overview

This case study documents a detection engineering investigation into a vishing-based social engineering attack resulting in suspected early-stage compromise.

The objective was to determine whether observed endpoint activity represented benign administrative behavior or post–initial access reconnaissance.

⸻

🧠 Scenario

A user was presented with a fraudulent “system infection” popup prompting immediate contact with a support number.

Upon interaction, the attacker leveraged social engineering tactics to establish trust and guide the user toward actions enabling system access.

This represents a user-driven initial access vector, rather than traditional malware-based intrusion.

⸻

🔍 Investigation Focus
	•	Identification of social engineering indicators
	•	Validation of malicious infrastructure (phone number, fake alert)
	•	Correlation with endpoint telemetry
	•	Behavioral analysis of process execution

⸻

🧪 Telemetry Analysis

Data Source
	•	Sysmon (Event ID 1 – Process Creation)

Observed Activity

Process: ipconfig.exe

At face value, this command is benign.

⸻

🔎 Contextual Analysis

When correlated with preceding social engineering activity:
	•	Execution timing aligns with user interaction
	•	Indicates potential network enumeration
	•	Suggests early-stage discovery activity

⸻

⚠️ Key Insight

The command wasn’t the signal.
The context was.

Isolated telemetry may appear benign.
Correlated telemetry reveals intent.

⸻

🧭 MITRE ATT&CK Mapping
	•	T1566 – Phishing (Vishing)
	•	T1204 – User Execution
	•	T1033 – System Owner/User Discovery
	•	T1016 – Network Configuration Discovery

⸻

📊 Assessment
	•	Initial Access: Confirmed (social engineering via scareware/vishing)
	•	Discovery Activity: Observed
	•	Persistence: Not identified
	•	Privilege Escalation: Not observed

🔐 Confidence Level: Medium

⸻

🧾 Rationale
	•	User interaction confirmed as the initial access vector
	•	Observed behavior aligns with early-stage reconnaissance patterns
	•	No evidence of persistence or privilege escalation at this stage

⸻

🚧 Recommended Next Steps
	•	Monitor for persistence mechanisms:
	•	Scheduled Tasks
	•	Registry modifications
	•	Validate potential remote access tool installation
	•	Review outbound network connections for C2 indicators

⸻

🐝 Beekeeper Insight

The threat does not begin with malware — it begins with manipulation.

Attackers don’t always break in.
Sometimes, they are invited in.

⸻

🔗 Related Case
	•	Operation Rogue Worker: Suspicious Discovery Activity

Demonstrates progression from:

Initial Access → Discovery → Potential Compromise Path

⸻
