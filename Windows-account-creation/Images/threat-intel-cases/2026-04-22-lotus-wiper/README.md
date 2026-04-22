# Lotus Wiper — Destructive Attack Against Venezuelan Energy Sector

> **Case opened:** 2026-04-22
> **Analyst:** Elizabeth ([Threat Huntress Lab](https://github.com/elizabethjean141/Detection-Engineering))
> **Source:** [Kaspersky / Securelist](https://securelist.com/tr/lotus-wiper/119472/) via [BleepingComputer](https://www.bleepingcomputer.com/news/security/new-lotus-data-wiper-used-against-venezuelan-energy-utility-firms/)
> **TLP:** GREEN · **PAP:** GREEN · **Severity:** High

---

## Executive Summary

A previously undocumented data wiper dubbed **Lotus Wiper** was deployed against the energy and utilities sector in Venezuela between late 2025 and early 2026. The malware leaves infected systems in an unrecoverable state. No extortion note is embedded — the attack is purely destructive, suggesting a nation-state or politically motivated actor rather than financial crime.

The sample was uploaded to a public analysis platform in mid-December 2025 from a machine in Venezuela, weeks before U.S. military action in the country in January 2026. Compilation date: late September 2025. Kaspersky stopped short of attribution but noted the timing coincided with "increased public reports of malware activity targeting the same sector and region."

---

## Attack Chain

### Stage 1 — Pre-Existing Domain Compromise

Kaspersky's assessment: *the attackers likely had knowledge of the environment and compromised the domain long before the attack occurred.* This is the most important detail in the entire case — the destructive phase is the last act, not the first.

### Stage 2 — First Batch Script (Network Coordination)

- Attempts to stop the **UI0Detect** (Windows Interactive Services Detection) service
- Target of UI0Detect indicates the script was built for Windows versions prior to 10 build 1803 — the attackers knew their victims ran legacy Windows
- Checks for a **NETLOGON share** and retrieves a remote XML file
- Cross-checks for a local file at `C:\lotus\` or `%SystemDrive%\lotus\`
- Uses a randomized delay of up to 20 minutes if NETLOGON is initially unreachable
- Executes the second batch script regardless of local-file presence

### Stage 3 — Second Batch Script (Environment Preparation)

Prepares the host for destructive activity:

- Enumerates local user accounts
- Disables cached logins
- Logs off all active user sessions
- Deactivates network interfaces (isolates the host)
- Runs `diskpart clean all` to wipe all logical drives
- Uses `robocopy` to recursively mirror folders and overwrite or delete contents
- Uses `fsutil` to create a file that fills all remaining free space, impairing recovery

### Stage 4 — Lotus Wiper Payload (Final Destruction)

- Enables all privileges in its access token
- Interacts with disks directly via IOCTL calls (below the file system layer)
- Retrieves disk geometry
- Clears USN (Update Sequence Number) journal entries
- Deletes all Volume Shadow Copy restore points
- Overwrites physical sectors with zeros — not just logical volumes
- Deletes all files for every mounted volume
- **Result:** host is unrecoverable

---

## Key Findings

- **Destructive, not financially motivated.** No ransom note, no C2 callback, no exfil — pure destruction.
- **Legacy Windows targeting.** UI0Detect focus indicates reconnaissance into victim environment ran well before execution.
- **LOLBAS-heavy.** Uses `diskpart`, `fsutil`, `robocopy` — binaries signed and trusted by Windows — to do most of the damage. The final payload only performs what batch scripts couldn't.
- **Operates below the file system.** IOCTL disk access + physical sector overwrite means file-system recovery tools won't work.
- **Inhibit Recovery is deliberate.** Filling disk free space before the wiper runs prevents shadow-copy and carving-based recovery.

---

## Patient Zero Question

As with every threat intel report — *how did someone notice first?*

The Kaspersky report doesn't specify. The sample was uploaded to a public platform from a machine in Venezuela. Someone — an incident responder, a defender, a researcher — pulled the artifact out of a compromised environment and shared it. That person was patient zero for the investigation.

**For defenders:** this is why detection engineering on **precursor behaviors** (UI0Detect manipulation, LOLBAS abuse, NETLOGON anomalies) matters far more than signatures on the final payload. By the time the wiper runs, recovery is already lost.

See also: [`mitre-coverage.md`](./mitre-coverage.md) · [`observables.md`](./observables.md) · [`detection-opportunities.md`](./detection-opportunities.md)

---

## Case Status

| Item | Status |
|---|---|
| Initial intake | ✅ Complete |
| MITRE ATT&CK mapping | ✅ Complete (see `mitre-coverage.md`) |
| Observables documented | ✅ Complete (see `observables.md`) |
| Detection gap analysis | 🟡 In progress |
| New rule drafting | 🟡 Planned |
| Rules deployed to Wazuh | ⬜ Not started |

---

## References

- Kaspersky / Securelist: *Lotus Wiper: a new threat targeting the energy and utilities sector* — https://securelist.com/tr/lotus-wiper/119472/
- BleepingComputer: *New Lotus data wiper used against Venezuelan energy, utility firms* — https://www.bleepingcomputer.com/news/security/new-lotus-data-wiper-used-against-venezuelan-energy-utility-firms/
- MITRE ATT&CK: https://attack.mitre.org/
