# PERSISTENCE & EVASION
30-10-26
 
---
## Contents
- Learning outcomes & resources
- Persistence
- Autoruns
- Registry, services and scheduled tasks
- Evasion
- Evidence and interpretation
- Practice task
---
## Learning Outcomes & Resources
- Describe common Windows **persistence** mechanisms and where evidence is recorded
- Use **Autoruns** to review autostart entries and interpret suspicious findings
- Distinguish **evasion techniques** from confirmed malicious activity
- Explain how to preserve and correlate endpoint evidence
- **Resources:** Microsoft Sysinternals Autoruns; *Practical Malware Analysis*; *Malware Analyst's Cookbook*; Week 3 Process Explorer / Process Monitor / Windows Security demos
---
## Persistence
- **Initial execution:** the program starts once (user action or event); it may terminate or be removed
- **Persistence:** a mechanism that makes software run again after a **reboot, login or trigger**
  - Legitimate applications use many of the same mechanisms
- **Why it matters in forensics:**
  - A suspicious executable may no longer be running when investigators arrive
  - Autostart config shows how a program was **intended** to re-launch
  - Correlate paths, command lines, users and timestamps before concluding
  - An autostart entry is **not proof** the file executed successfully
### Common Persistence Categories
| User and logon | System and triggers |
|---|---|
| Startup folders | Windows services |
| `Run` / `RunOnce` registry values | Scheduled tasks |
| Logon scripts, shell configuration | WMI permanent event subscriptions |
| | Other extension and application autoruns |
 
### Locations (analyst reference)
- **User-level:** `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`; `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup` (the older slides used `MSINIT32.BAT` as an example startup file)
- **Machine-level:** `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`; `HKLM\SYSTEM\CurrentControlSet\Services`
  - Machine-wide persistence may need **elevated privileges**; review permissions, paths and event evidence
- **Services:** may start at boot or on trigger; inspect executable paths and service accounts
- **Scheduled tasks:** run at logon, on a schedule or on other triggers; investigate names, actions, authors, triggers, creation/modification records
  - Correlate with Task Scheduler logs, Security logs and process events
---
## Autoruns (Sysinternals)
- Shows programs configured to run at startup, login and many other trigger points
- Compare **user-level vs system-level** entries across tabs
- Inspect **image path, publisher, digital signature, timestamp, command line**
- An unsigned entry is a **clue, not proof** of malware
- **Export observations for documentation before modifying anything**
- **Practical (clean VM):** open in an approved analysis VM; inspect **Logon, Scheduled Tasks, Services**; identify familiar Microsoft/driver/app entries; note unusual paths; **keep a baseline** before examining later changes
---
## Evasion
| Passive / concealment | Active / interference |
|---|---|
| Packing or obfuscating code | Trying to impair security controls |
| Misleading names, descriptions, file metadata | Interfering with monitoring or investigations |
| Using legitimate Windows components | Re-launching terminated components |
| Memory-resident execution (fewer disk artefacts) | Altering process behaviour to complicate analysis |
 
- **Active evasion (defensive view):** older malware tried to stop AV services or kill monitoring processes
  - Modern **tamper protection** and permissions block many attempts
  - Look for service-control events, process termination, protection alerts, config changes
  - Distinguish a **failed attempt** from an actual disablement using endpoint telemetry
- **Watchdog / buddy processes:** one process monitors another and restarts it
  - Can resemble legitimate reliability tooling, so context matters
  - Look for repeated process creation, parent-child relationships, scheduled triggers
  - Preserve process timelines and file metadata; don't delete artefacts immediately
- **Living-off-the-land binaries (LOLBins):** legitimate utilities (e.g. `rundll32.exe`) used in suspicious chains
  - Filename alone doesn't decide; check **full command line**, the referenced DLL/file, **parent process**, user context
  - Compare with expected application activity using trustworthy telemetry
- **Process injection:** malicious code runs inside another process, so a process name doesn't identify its code
  - Evidence: unexpected loaded modules, memory regions, anomalous access events
  - Test conclusions against **memory captures** and telemetry
---
## Evidence & Interpretation
| Observed artefact | Still to establish |
|---|---|
| Autoruns entry or task definition | Did it actually run? |
| Service installation record | When, and under which account? |
| Suspicious command-line event | What program/DLL was invoked? |
| Defender / EDR alert | What other evidence supports the sequence? |
 
---
## Summary
- Persistence is a **re-execution mechanism**, not a malware label
- Autoruns enumerates many Windows autostart locations
- Evasion = concealment, interference, or abuse of legitimate tools
- Strong conclusions need **corroborated host evidence** and accurate timelines
---
## Independent Study
- Run Autoruns on a clean personal lab VM; document baseline entries
- Compare service, task and Run-key evidence using published forensic references
- Identify artefacts that would help confirm suspected evasion
- Keep malware and analysis off University-managed computers
- **Persistence triage scenario:** you find an unfamiliar Run-key entry in a forensic image
  - Record its path and account; **don't execute** the target
  - Identify supporting logs, file metadata and other artefacts
  - Give **two benign alternatives** to your first suspicion
  - Write a brief finding separating **observations, hypotheses and conclusions**
