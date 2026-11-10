# FILE ATTRIBUTES & ADS STREAMS
08-10-26
 
---
## Contents
- File attributes (Windows, Linux, BSD)
- Extended attributes
- Tasks: Windows demo, Linux demo, hidden folders, undeletable file
- Alternate Data Streams (NTFS)
---
## Resources
- oddvar.moe: *Putting data in Alternate Data Streams and how to execute it* (2018)
---
## File Attributes
- **Properties (characteristics) defined by the file system**
- Support and features depend on the **file system**, not the overall OS version
- Think of them as **metadata** about a file/folder (**not** including permissions)
  - Different from other metadata such as created/modified timestamps
  - Permissions are excluded to abstract them away, for more security
- Each attribute has 2 states: **set** or **unset** (on/off)
### Common Windows Attributes
| Flag | Attribute |
|---|---|
| `R` | Read-only |
| `A` | Archive |
| `S` | System |
| `H` | Hidden |
 
### Commands by OS
| OS | List | Change |
|---|---|---|
| Windows | `attrib` | `attrib` |
| Linux | `lsattr` | `chattr` |
| BSD-based (Mac / *BSD) | `ls` | `chflags` |
 
---
## Extended Attributes
- Designed to **extend** the traditional file system attributes
- "Associates metadata with a file not historically supported in the file system"
  - e.g. iOS uses extended attributes to store a flag for whether a file should sync to iCloud
- **Why do we need them?**
  - Can hold info unrelated to the FS itself
  - Allows metadata such as digital signature, author, cursor position in a text file
  - Allows bash mode in Win 10 to work via ADS in NTFS
---
## Tasks & Demos
 
### Windows Demo (VM)
- Create a test file: `echo testing > test.txt`
- View files with `dir` and `attrib`
- Make something hidden:
  - `mkdir secret`
  - `attrib +h secret`
### Linux Demo
- `touch file`, then `ls -l file` -> `-rw-rw-r--. 1 cyber cyber 0 Nov 24 13:11 file`
- `lsattr file` -> `--------------e---- file`
- **The `e` flag:** the file uses **extents** for mapping blocks on disk; it can't be removed with `chattr` (check `man lsattr`)
### Creating a Hidden Folder in Linux
- Prefix the name with a dot: `mv my_stash .my_stash`
- (`mkdir secret` is the plain, non-hidden creation shown on the slide)
### Task (10 mins): Undeletable File in Linux
- Create a file that **cannot be deleted**
- **Hint:** think about extended attributes
  - Answer: the **immutable** flag, `chattr +i file` (needs root); remove with `chattr -i file`
---
## ADS (Alternate Data Streams)
- An **NTFS** feature
- Every file on NTFS has at least one stream, called **`:$DATA`**
- ADS can put data into a file **without affecting its size**, or give an application temporary storage
### Task 1: Data in a Stream
- `echo "backup settings: 123" > myprogram.exe:config.txt`
- `notepad.exe info.txt:extracontent.txt`
### Task 2: Executables in a Stream
- Used to be possible to hide and run an executable:
  - `type C:\windows\system32\notepad.exe > c:\windows\system32\calc.exe:notepad.exe`
  - `start c:\windows\system32\calc.exe:notepad.exe`
- **No longer works** on modern Windows (see oddvar.moe article)
---
## Summary
- You should now be familiar with how file attributes and ADS streams work
