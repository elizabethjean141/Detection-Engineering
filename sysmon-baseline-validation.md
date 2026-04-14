# Sysmon Process Creation Baseline Validation (Event ID 1)

## Overview
This case was created to validate a detection engineering baseline using Sysmon Event ID 1 (process creation) and confirm expected system behavior in a controlled lab environment.

The goal was to analyze process execution, validate parent-child relationships, and establish a behavioral baseline for future anomaly detection.

---

## Scenario
Simulated normal administrative activity:

- ipconfig.exe
- whoami.exe
- systeminfo.exe

These commands were executed to observe process creation patterns and expected system behavior.

---

## Analysis Approach

- Reviewed Sysmon Event ID 1 logs
- Validated parent-child relationships:
  - cmd.exe → child processes
  - powershell.exe → administrative commands
- Compared activity against known-good baseline behavior
- Mapped activity to MITRE ATT&CK Discovery techniques:
  - T1016 – Internet Connection Discovery
  - T1033 – System Owner/User Discovery
  - T1082 – System Information Discovery

---

## Findings

- Observed processes aligned with expected administrative behavior  
- Parent-child relationships were consistent with legitimate execution chains  
- No anomalous execution patterns identified  

👉 **Conclusion: No indicators of compromise were detected**

---

## Why This Matters

This baseline will be used to:

- Detect abnormal process execution
- Identify suspicious parent-child relationships
- Reduce false positives in detection rules

Detection starts with understanding normal 
