# FILE SYSTEM, PARTITIONS & HOW DATA STORED
08-10-26

---
## Contents
- 

---
## Data, Information & Metadata
- **Data:** raw facts and figures that need processing to be useful
  - e.g. `LS, TSP, TAS025, ELTON`
- **Information:** processed data with structure and context, so it has meaning
  - e.g. the above becomes: Lee Speakman, Ashton Building Room 035, Thornton Science Park, Elton
- **Metadata:** data that provides information about other data ("data about data")
  e.g. a PPT's author, date created, etc. 

---
## Data Structures & Blocks
- Computers handle many data types, so data must be organised to manage large amounts efficiently 
- Built from primitive types: integer, char, bool, float
- **Data block:** a sequence of bits of a defined block size, read/written all at once
  - Blocking = data into blocks; de-blockig is the reverse 
  - Example: BitBlt -> buffering

---
## Block Devices 
- **Block special file / block device:** a file that refers to a device, hiding the hardware characteristics. Pseudo devices such as `/dev/null` exist too
- Block device: read/write in whole blocks (at a block boundary)
- Character device: read/write at any size (not necessarily a single char)
- Linux: everything is a file or dir. Look in `/dev`
  - `/dev/sda` (disk), `/dev/null`, `/dev/urandom`
- **Task:** find other devices in `/dev` on your Linux VM

---
## Partitions
- A disk must be divided up to be used (1 more divisions). Default state is unallocated space
- **Partitition table:** size and location of all partitions on the disk

### MBR (legacy)
- Master Boot Record. Contains the boot loader (GRUB, NTLDR, LILO, etc.)
- Max 2TB per partition
- Primary: max 4
- Extended: max 1, used to create secondary partitions
- Logical (secondary): multiple can sit inside the extended partition, which gets around the 4 limit

### GPT (newer)
- GUID partition table. Each partition has its own GUID
- Multiple copies of the partition table on disk plus CRC checks, so much more redundant
- Less compatible than MBR, but GPT + UEFI are replacing MBR + traditional BIOS

---
## Disk Internals
- **Track:** a ring division of the platter
- **Geometric sector:** abstraction of the disk (a wedge)
- **Sector:** a track sector, the smallest physical unit
- **Cluster:** a contiguous run of sectors used by the OS file system
- Zone density recording: fewer sectors per track as you move inwards
