# PHYSICAL STORAGE INTERFACES & STORAGE MEDIUMS
08-10-26
 
---
## Contents
- Learning outcomes & resources
- Interfaces: ATA, PATA, SATA, SCSI, SAS
- Mediums: HDD, flash memory, SSD, flash drive, SD card, CD/DVD, Blu-ray
- Drive longevity
---
## Learning Outcomes & Resources
- Understand the different **interfaces** these mediums use to connect to the computer
- Develop a working knowledge of common **storage mediums** and how they work
- **Resources:**
  - Sammons, J. (2014). *The Basics of Digital Forensics* (2nd Ed). Syngress (aka BODF)
  - Hard disk images by Lucas O'Mahony (Nov 2018) -> Moodle folder `Hard Disk Pictures`
  - Moodle video: *What's inside a Hard Drive & Solid State Drive? (HDD/SSD)*
---
## 1. Interfaces
**Covered:** ATA, PATA, SATA, SCSI, SAS, M2 (M.2)
 
### ATA (Advanced Technology Attachment)
- Surpasses **IDE** (Integrated Drive Electronics)
  - IDE's main contribution: the controller is built into the drive itself, so the motherboard only needs a simple circuit
- ATA is a **standard**: a blueprint / set of rules
### PATA (Parallel ATA)
- An implementation of the ATA standard
- Data sent **16 bits at a time**, full duplex
- Today, PATA is called "IDE" and IDE is called PATA (the legacy technology)
- CD/DVD drives also used this connector
### SATA (Serial ATA)
- Successor to PATA, **incompatible** with PATA
- Faster data rate (PATA 133 MB/s vs SATA 1 at 150 MB/s) and a smaller connector
- **Backwards compatible** with older SATA versions; speed is capped by the controller
| Revision | Speed | Realistic speeds | Release date |
|---|---|---|---|
| SATA 1 | 1.5 Gb/s | 1-150 MB/s | 7 Jan 2003 |
| SATA 2 | 3 Gb/s | 151-300 MB/s | Apr 2004 |
| SATA 3 | 6 Gb/s | 301-600 MB/s | 27 May 2009 |
 
- e.g. a 2.5" SATA SSD: sequential read max 540 MB/s, sequential write max 520 MB/s
### SCSI (Small Computer System Interface)
- A **standard** for defining the connection medium
- Can attach **multiple devices to 1 port**
- Variants: **SAS**, **iSCSI** (Internet SCSI, IP-protocol based)
### SAS (Serial Attached SCSI)
- Much faster than SATA, with little physical difference
- Up to **15k RPM**
- Used in **enterprise servers**
- Backwards compatible with SATA 2/3
  - SATA drive **can** connect to a SAS backplane
  - SAS drive **cannot** connect to a SATA backplane
---
## 2. Mediums
 
### HDD (Hard Disk Drive)
- Invented by **IBM in 1954**
- Consumer: 5400 / 7200 RPM; enterprise grade: 10k RPM
- **Magnetic-coated platters** for seeking on the disc
- Internals covered via diagrams (ref: tierradatarecovery.co.uk) and the Moodle demo/video
### Flash Memory
- Improved upon **EEPROM** (Electrically Erasable Programmable Read-Only Memory)
  - EEPROM writes at **byte level**
  - Flash writes data in a **block** (remember this)
- Both are **non-volatile**: data persists with no power
- **Why not use flash instead of DRAM?**
  - DRAM doesn't need to erase memory before reuse
  - DRAM reads multiple bytes at once
- Flash: write in a block, **page access** = a block read
- **Flash wearing:** trying to keep each cell alive as long as possible
- **XLC (X Level Cell):** generally, the higher X is, the more storage but the slower the NAND is to access
  - **SLC** (Single): 1 bit per cell, fast access, common in fast SSDs
  - **MLC / DLC** (Multi / Double)
  - **TLC** (Triple): most common in 2024
  - **QLC** (Quad)
### NAND and NOR
- NAND is a type of flash memory; the other common type is **NOR** (less common)
- NAND stores large amounts of data, so it is ideal for large files
- NAND reads in a block (page access); in both NAND and NOR a block can be multiple kilobits
- NAND is **slow for reading**, a bad idea for quick access (DRAM is bit-level addressed, so much faster)
- NAND has a durable lifespan
### SSD (Solid State Drive)
- Built on NAND technology
- Components:
  - **A:** NAND-based flash memory
  - **B:** volatile memory (RAM)
  - **C:** controller, containing the firmware, connects A and B
### TRIM
- An **ATA command**, firmware built into the SSD
- Needs: UEFI/BIOS -> SSD -> **AHCI mode** (must be on for TRIM to be enabled)
- Built into nearly all modern OSes
- Helps prevent SSD degradation by freeing unused space (making it free for the OS) and spreading data around
### Form Factors
- **3.5"**: full-size HDDs in desktops / servers
- **2.5"**: SSD size, and laptops with PATA or SATA HDDs (PATA has fewer pins)
- **mSATA**: Mini SATA, mini-PCI Express; connects to the SATA controller
### Flash Drive / Pen Drive / USB Stick
- Uses NAND like an SSD, but with different quality flash NAND, controller and interface
- Typically a **USB 2/3** interface
### SD Card
- Based on a type of flash memory
- Rated in **Classes** for performance
- Fast read times, slow write times (depends on the controller used)
### CD / DVD
- Optical (laser) used to read/write bits; typically 12 cm diameter
| | CD | DVD |
|---|---|---|
| Structure | 1 polycarbonate layer | 2 polycarbonate layers |
| Capacity | 700 MB | 4.7 GB (single-sided, single-layer, common); 8.5 GB (single-sided, double-layer) |
| RPM | 500 | ~1500 |
 
### Blu-ray
- Newer standard for storing large files on disc; successor of DVD
| Size | Layer |
|---|---|
| 25 GB | single |
| 50 GB | dual |
| 100 / 128 GB | BDXL format |
 
---
## Drive Longevity
- HDDs, SSDs etc. **will die someday**
- Factors in preservation:
  - **MTBF:** mean time between failure
  - Quality of flash / components
  - **Arrhenius equation** (temperature-driven failure rates)
  - Wear levelling (slide says "wear level tearing"), and TRIM
  - Physical damage
