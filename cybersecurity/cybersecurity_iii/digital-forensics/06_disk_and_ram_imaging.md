# DISK & RAM IMAGING: PRACTICAL EVIDENCE ACQUISITION
15-10-26
 
---
## Contents
- Objectives & resources
- Lab setup: VMs and shared folders
- Disk imaging with `dd`
- FTK Imager
- Imaging tool comparison & specialist recovery
- Verifying image integrity
- RAM acquisition with Magnet DumpIt
- Analysing RAM with Volatility 3
- Summary & practical tasks
---
## Objectives & Resources
- **Competency 1:** evaluate collection tools and systems
- **Competency 2:** capture and audit the state of a computer
- Tools on Moodle: **FTK Imager** and **Magnet DumpIt** (Oct 2026)
- Video: *Disk Imaging using FTK Imager* (2 Jan 2024), methods still the same
- Further reading: Sammons (2014), *The Basics of Digital Forensics*; forensics.wiki
---
## Lab Setup
 
### Virtual Machines (VirtualBox)
- **Ubuntu 26.04.1 LTS** forensic VM (i3, Rofi, Thunar, LXTerminal + forensic/dev tools)
- **Windows 11** forensic VM
- Student login for both: `cyber` / `cyberp4ss`
- Import each VM, configure the host shared folder, and **take a snapshot** before practical work
### Shared Folders
1. On the Windows **host**, create `C:\CO6077-Shared` (at least 2 GB free)
2. For **each** VM: Settings -> Shared Folders -> add the same host folder, enable **Auto-mount**
3. Ubuntu: `findmnt -t vboxsf`; common mount `/media/sf_CO6077-Shared`
4. Windows 11: open `\\VBOXSVR\CO6077-Shared` in File Explorer
5. Ubuntu: `sudo usermod -aG vboxsf "$USER"`, then log out and back in
6. Test **reading and writing** from both VMs (guest paths differ)
### Ubuntu Lab Preparation
- Power off Ubuntu; in VirtualBox Storage add a **new blank, dynamically allocated 1 GB disk**
- Start Ubuntu, open LXTerminal (i3: `Win + Enter`): `lsblk -o NAME,SIZE,TYPE,MOUNTPOINTS`
- Identify the new disk **by size**, never assume it is `/dev/sdb`
- Never modify the Ubuntu system disk or any real evidence device
### Create & Populate a Disposable FAT Disk
1. Identify the 1 GB disk: `lsblk -o NAME,SIZE,TYPE,MOUNTPOINTS`
2. `sudo fdisk /dev/sdX` (that disk **only**)
3. In fdisk: `g` (GPT), `n` (new partition, accept defaults), `w` (write)
4. Format: `sudo mkfs.vfat /dev/sdX1`
5. Mount: `sudo mkdir -p /mnt/forensic-test && sudo mount /dev/sdX1 /mnt/forensic-test`
6. Test data: `echo "CO6077 test" | sudo tee /mnt/forensic-test/notes.txt`
7. Finish: `sync && sudo umount /mnt/forensic-test`
- **Warning:** `sdX` is a placeholder; verify the 1 GB disk before `fdisk` / `mkfs`
---
## Disk Imaging
 
### Acquire a Raw Image with `dd` (Ubuntu)
1. Verify the host share has space and the test partition is **unmounted** (`lsblk`)
2. Image the **whole disk**, not `/dev/sdX1`:
   `sudo dd if=/dev/sdX of=/media/sf_CO6077-Shared/testdisk.dd bs=4M iflag=fullblock status=progress conv=noerror,sync`
3. `sync` to finish writing
4. Record SHA-256: `sha256sum .../testdisk.dd | tee .../testdisk.dd.sha256`
5. Verify on the host; record disk identity, command, time and errors
- **`dd` limitations:** no metadata gathering, error correction, piecewise hashing, or user-friendly interface
- If `dd` reports permission/space errors: check folder access and free space, then repeat safely
### Workflow A: Ubuntu Disk -> Windows FTK Imager
1. Ubuntu: add the disposable 1 GB disk; create GPT partition + FAT filesystem
2. Create sample files, then unmount
3. Image the whole disk with `dd` into the host shared folder; create the SHA-256 record
4. Windows 11: find `testdisk.dd` in the same shared folder
5. FTK Imager -> File -> Add Evidence Item -> Image File -> `testdisk.dd`
6. Inspect the file system and test files; **rehash after transfer**; retain the original unchanged
### FTK Imager
- GUI **acquisition, review and verification** (Windows)
- Use the package supplied on Moodle, in the Windows 11 teaching VM
- In this practical, **open** the existing `testdisk.dd` (Ubuntu created the image)
- Expand the evidence tree; view the partition and files made in Ubuntu
- Check the image SHA-256 with a **separate** hash tool
- Don't edit the original image; use only designated lab disks and non-sensitive test evidence
- Screenshots on the slides are historical; menus may differ
### Comparing Imaging Tools
| Tool | Notes |
|---|---|
| **FTK Imager** | GUI acquisition, review and verification (Windows) |
| **dd** | General-purpose raw imaging utility (Linux / Unix-like) |
| **dcfldd** | Forensic-oriented `dd` derivative (availability varies) |
- Consider output formats, verification, logs and **source write protection**
- Select a tool according to the device and **defensibility** requirements
### dcfldd
- Derived from `dd`; supports on-the-fly hashing, logs, split outputs, multiple destinations
- Example: `dcfldd if=/dev/sourcedrive hash=md5,sha256 hashwindow=10G md5log=md5.txt sha256log=sha256.txt hashconv=after bs=512 conv=noerror,sync split=10G splitformat=aa of=driveimage.dd`
- **Warning:** the reference warns about faults on damaged media; don't use blindly on failing drives
### Specialist Recovery (Difficult / Failing Media)
- Hardware-assisted; specialist tools and expertise may be needed
- **ACE Lab PC-3000:** HDD, SSD, flash and RAID recovery hardware/software
  - Discuss: how might device faults or firmware failures affect evidence collection?
- **MRT:** alternative to PC-3000
  - Consider functionality, compatibility, support, audit trail, evidence preservation
- **Rusolut:** NAND reconstruction and **chip-off** recovery; flash memory tools
  - Physical flash recovery and logical file-system examination solve **different problems**
---
## Verifying Image Integrity
- Calculate **SHA-256** after acquisition; **rehash the same image after copying**
- Matching hashes show the copies are **identical**; they do **not** prove the source acquisition was error-free
- Historic screenshot shows an older hash; prefer **SHA-256**
- Keep notes: source device, tool version, times, errors, chain of custody
---
## RAM Acquisition
 
### Why Capture RAM?
- Holds running processes, command history and network activity
- **Volatile**: may disappear on shutdown
- Live acquisition **modifies the system** and must be documented
- Name files consistently; record time, host and tool
### Magnet DumpIt: Acquire RAM (Windows VM)
1. Start the Windows 11 VM; check shared-folder access
2. Place DumpIt in an approved lab location (local work folder or shared folder)
3. Run as **administrator**; confirm actual output location and capacity **before** capture
4. Capture RAM; wait for completion; record filename, tool version, time, warnings, errors
5. If captured locally, **copy** the finished image to the host shared folder
6. PowerShell: `Get-FileHash -Algorithm SHA256 "C:\path\to\actual-capture.raw"`
7. Preserve the original; live acquisition changes the running system
### Preserve & Transfer
- Record computer name, Windows build, DumpIt version, time, errors
- Record source SHA-256 with `Get-FileHash`
- Ubuntu: copy the capture from the shared folder to a local analysis directory
- `sha256sum` the working copy and compare with the Windows hash
- Use the real output filename and format (don't assume `memory.raw`)
---
## Analysing RAM: Volatility 3
- **DumpIt acquires** Windows memory; **Volatility 3 analyses** the image in Ubuntu
- Copy the image locally and **verify SHA-256 first**
- Commands (replace with actual filename):
  - `vol -f /path/to/capture.raw windows.info`
  - `vol -f /path/to/capture.raw windows.pslist`
  - Also try `windows.pstree`, `windows.cmdline`, `windows.netscan` if compatible
  - If installed from source, use `python3 vol.py` instead of `vol`
- Not all Windows 11 builds/plugins work identically; record unsupported plugins/errors
- **Versions:** Volatility 3 is the one to learn; don't mix Volatility 2 commands/plugins into a Vol 3 walkthrough
- Choose plugins supported by the captured OS and data format
- Resources: volatility3.readthedocs.io, github.com/volatilityfoundation/volatility3, HackTricks cheat sheet (Vol 2 and 3); compare plugin output with other evidence where possible
### Workflow B: Windows DumpIt -> Ubuntu Volatility 3
1. Start Windows 11 VM; check share access and free space
2. Capture live memory with DumpIt as administrator
3. Wait for completion; copy to the host share; calculate SHA-256
4. Ubuntu: copy the capture locally and verify the same SHA-256
5. `vol -f /path/to/actual-capture.raw windows.info`
6. Try `windows.pslist` and other supported plugins; record findings and limitations
### Practice Memory Images
- Prefer instructor-provided, authorised lab captures
- Public dumps may be old, unavailable or OS-specific; record source, OS version, format, expected plugin support
- Extra: Volatility Foundation historical memory samples (GitHub wiki); OtterCTF memory forensics walkthrough
---
## Summary & Practical Tasks
- Identify, image and verify a test storage device
- Understand the difference between **disk** and **live RAM** acquisition
- Use FTK Imager and Magnet DumpIt in a controlled environment
- Document integrity, limitations and chain of custody
- **Tasks:**
  - Prepare a disposable test disk in your VM
  - Acquire and hash a disk image; inspect it with FTK Imager
  - Capture RAM with DumpIt where your VM supports it
  - Hash, label and document both outputs
  - Compare the evidence recoverable from disk and RAM
