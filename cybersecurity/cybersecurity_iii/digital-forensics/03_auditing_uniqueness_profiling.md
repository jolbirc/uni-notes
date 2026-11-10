# AUDITING, UNIQUENESS & PROFILING
08-10-26
 
---
## Contents
- Learning outcomes & resources
- Auditing: logging, environment variables, Event Viewer, registry, Active Directory
- Uniqueness: hardware IDs, MAC address, UUID/GUID
- Profiling: cookies, browser fingerprinting
- Homework
---
## Learning Outcomes & Resources
- Review knowledge of auditing systems
- Establish a fundamental grasp of: **auditing**, **uniqueness**, **profiling a target**
- **Resources:**
  - Sammons, J. (2014). *The Basics of Digital Forensics* (2nd Ed). Syngress
  - Ligh, M. H. (2014). *The Art of Memory Forensics*. Wiley
  - Microsoft: *Appendix L: Events to Monitor* (Windows Server AD DS docs)
- **Forensics review (class discussion):** experience with forensics, what DF is, what you want from the module
  - Typical answers: recovering deleted files, recovering from broken devices, disk and memory imaging/analysis, file and process analysis, network analysis, metadata extraction
---
## 1. Auditing
- **Security auditing:** periodic review of a system's / network's security status
- **Logging:** recording system events in special files, kept for a predetermined duration
  - Linux: `/var/log/`; Windows: Event Viewer
  - Happens throughout the stack: client, server, infrastructure
### Environment Variables
- **Windows:**
  - `set` shows all environment variables
  - `echo %env_variable%` shows one
  - `%SYSTEMROOT%` = `C:\Windows` (on most systems)
- **Linux:** `env`
### Windows Auditing Tools
- **Event Viewer**, **Log Parser 2.2**
- Event logs are saved as **`.evtx`**
- Stored in:
  - `%SYSTEMROOT%\System32\winevt\Logs`
  - `%SYSTEMROOT%\System32\config`
### Event IDs
- Every event has a **unique ID**; IDs differ between Windows OS versions
- Events can be filtered by ID. Examples:
  - **4720:** user account was created
  - **4724:** attempt made to reset an account's password
  - **4738:** user account changed (password change)
- Refs: ultimatewindowssecurity.com security log encyclopedia, malwarearchaeology.com cheat sheets, Microsoft *Events to Monitor*
### Registry (Windows)
- View with `regedit.exe` / `reg.exe`
- Files: `%SYSTEMROOT%\System32\config`; `%USERPROFILE%\ntuser.dat` for HKCU (`C:\Users\username`)
- The underpinning **database** for:
  - OS configuration
  - App settings
  - Credentials (**SAM** db)
- **Structure:** Hive -> Key -> Sub Key -> Value
#### Registry Hives
| Hive | Purpose |
|---|---|
| `HKEY_CLASSES_ROOT` | File associations, file system |
| `HKEY_USERS` | Sub-keys for each user; per-user settings, works with HKCU |
| `HKEY_CURRENT_USER` *(dynamic)* | OS checks who is logged on and pulls info from their sub-key in HKEY_USERS |
| `HKEY_LOCAL_MACHINE` | Computer-specific settings |
| `HKEY_CURRENT_CONFIG` *(dynamic)* | Pointer to `HKEY_LOCAL_MACHINE\Config` |
 
- **Dynamic** hive: changes depending on which user is logged in
#### Exporting & RegJump
- Registry exports are **`.reg`** files, e.g.
  - `Windows Registry Editor Version 5.00`
  - `[HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip]`
  - `"Path64"="C:\\Program Files\\7-Zip\\"`
- **RegJump** (Sysinternals): opens regedit at a given path
  - `regjump.exe HKEY_LOCAL_MACHINE\SOFTWARE\7-Zip` (cd to its folder first)
  - `set path=%path%;C:\Users\user\Downloads\RegJump` adds it to PATH, but is **not remembered** once cmd is closed. For a permanent change, edit PATH via Advanced settings
### Active Directory (AD)
- Objects: **Groups, Users, Organizational Units (OUs)**
- **`Ntds.dit`**: the domain database (default `%systemroot%\NTDS`)
- Domain controller uses event logs for password auths, replication attempts etc.
- **ADDS** (Active Directory Domain Services) = the role
- **AD Users and Computers** = management tool (also ADFS)
- More on AD later for the assignment
---
## 2. Uniqueness
- Computers have various "unique" sources, and these can often be **spoofed**
| Source | Windows command |
|---|---|
| MAC address | `getmac` or `ipconfig /all` |
| Motherboard serial | `wmic baseboard get serialnumber` |
| BIOS serial | `wmic bios get serialnumber` |
| Hard drive serial | `wmic path win32_physicalmedia get SerialNumber` |
 
### MAC Address (Media Access Control)
- Unique hardware ID of the **NIC**
- Format: `MM:MM:MM:SS:SS:SS` or `MM-MM-MM-SS-SS-SS`
  - **MM** = Manufacturer / **OUI** (Organizationally Unique Identifier), first 3 bytes, identifies the NIC maker
  - **SS** = Specific device, last 3 bytes, the unique part
- Each part is 8 bits (1 byte); 6 bytes in total
### UUID / GUID
- **UUID:** Universally Unique Identifier, a standard for identification **without central management**
- **GUID:** Microsoft's real-world implementation of the UUID standard (endianness is reversed, among other things)
- Uniqueness is **not guaranteed**, more a best effort
- Windows: `wmic csproduct get UUID` (stays the same, unique to your machine)
- Linux: disks also have UUIDs
---
## 3. Profiling
 
### Cookies
- A piece of text used by a website to identify a user, set via the `Set-Cookie` HTTP header
- **Session cookies:** temporary, no expiry date, expire when the browser closes
- **Persistent cookies:** expire after a set time or date (if the website sets one)
  - Used for remembering login sessions, **tracking**, etc.
- **Super cookie:** uses a TLD as origin; blocked by most browsers now
- **Countermeasures:**
  - EU law compliance; right to remove cookies
  - Blockers: uBlock Origin, Adblock Plus
### Profiling Using a Web Browser (Fingerprinting)
- Fonts
- Plugins
- User agent
- Screen resolution
- HTML5 canvas
- **Demo sites:** coveryourtracks.eff.org (previously Panopticlick), browserleaks.com/canvas
  - Discuss: what results, anything strange for tracking, how unique are you?
---
## Homework
- Evaluate your web footprint on your own devices
- Read `github.com/Psmths/windows-forensic-artifacts` and look around your system for places to harvest information
- Check the other hyperlinks in this week's slides for resources
- Play with **Volatility 3**: get used to the basic commands
