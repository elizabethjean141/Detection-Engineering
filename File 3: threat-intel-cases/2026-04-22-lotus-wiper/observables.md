# Lotus Wiper — Observables & Hunt List

## File System Artifacts

| Path | Meaning |
|---|---|
| `C:\lotus\` | Local staging directory |
| `%SystemDrive%\lotus\` | Alternate staging path |
| `C:\lotus\cmd.exe` | System binary copied to unusual location (strong IOC) |
| `C:\lotus\*.bat` | First-stage and second-stage batch scripts |
| `C:\lotus\*.xml` | Remote config cached locally |

## Process & Command-Line Behaviors

| Command | Purpose in Campaign |
|---|---|
| `net stop UI0Detect` / `sc stop UI0Detect` | Disable Interactive Services Detection |
| `diskpart /s` or interactive `clean all` | Wipe all logical drives |
| `robocopy <src> <dst> /MIR` | Recursively overwrite/delete directory contents |
| `fsutil file createnew` | Fill remaining disk space to prevent recovery |
| `wbadmin delete catalog -quiet` | Delete backup catalog |
| `vssadmin delete shadows /all` | Delete Volume Shadow Copies |
| `net user` | Local account enumeration |
| `logoff` or `shutdown /l` | Force session logoff |

## Network Indicators

- Unexpected access to `\\<DC>\NETLOGON\*.xml`
- Scripted randomized delays (0–20 minute range) against NETLOGON reads
- Sudden widespread network-interface disable events across multiple endpoints

## Service & Registry Indicators

- Stopped: `UI0Detect` (only present on Windows < 10 build 1803)
- Registry modification: `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\CachedLogonsCount`
- Volume Shadow Copy Service: stopped or VSS deletion events

## Hunt Queries (Conceptual)

### Wazuh Agent — Sysmon FileCreate for `C:\lotus\`
```
sourcetype=wazuh AND location="Microsoft-Windows-Sysmon/Operational" AND eventID=11 AND targetFilename CONTAINS "\\lotus\\"
```

### Wazuh Agent — Suspicious LOLBAS co-execution
```
Within 5 minutes on a single host:
  ProcessCreate events matching (diskpart.exe OR fsutil.exe OR robocopy.exe)
  Count >= 3 distinct binaries → alert
```

### Windows Security — UI0Detect service stop
```
Security EID 7036 where Service = "Interactive Services Detection" AND Action = "stopped"
```

### Windows Security — Mass logoff
```
Security EID 4634 (logoff) OR EID 4647 (user initiated logoff)
Count >= 10 within 60 seconds on a single host
```
