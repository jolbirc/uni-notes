# DIGITAL EVIDENCE ACQUISITION & THE SILK ROAD INVESTIGATION
15-10-26
 
---
## Contents
- Learning aims & sources
- Acquisition responsibilities (technician vs examiner)
- Stage 1: identification & preservation
- Stage 2: acquisition & examination
- Silk Road case study
- Key takeaways & homework
---
## Learning Aims & Sources
- Explain **identification, acquisition and preservation** of digital evidence
- Distinguish **forensic technician** and **examiner** responsibilities
- Examine the Silk Road investigation using **primary sources**
- Critically evaluate acquisition decisions and evidence integrity
- Module competencies: **1** (collection tools) and **2** (capture and audit)
- **Sources:**
  - Sammons (2014), *The Basics of Digital Forensics* (2nd ed.) (BoDF)
  - Silk Road court exhibits and transcripts: antilop.cc/sr
  - YouTube: *Silk Road Case - The Real, Untold Story* (2020, re-uploaded Nov 2022)
  - YouTube: Jared Der-Yeghiayan interview (11 Sep 2026)
  - Channel 4: *Manhunt: Unabomber* (optional)
---
## Acquisition Responsibilities
| Role | Responsibilities |
|---|---|
| **Forensic technician / first responder** | Identify, photograph, document and preserve potential evidence; capture **volatile** data when justified; create **forensic images** |
| **Forensic examiner / analyst** | Verify integrity; examine **working copies, not originals**; analyse disk, memory, file and network artefacts |
 
- Roles can overlap; procedures depend on circumstances
- **Image:** a bit-for-bit forensic copy of a storage medium (whiteboard demo in class)
---
## Stage 1: Identification & Preservation
- Record the scene, devices, times and acquisition decisions
- Identify **power state**, **encryption** and **volatile evidence** risks
- Record device identifiers and **chain of custody**
- Use appropriate **write protection** for offline media
- Avoid altering a live system without documenting why
---
## Stage 2: Acquisition & Examination
- Choose **live or offline** acquisition based on risk and evidence needs
- Capture and preserve data with **documented tool versions**
- Record **hashes** (preferably **SHA-256**), timestamps and logs
- **Verify image integrity** before analysing a working copy
- Document limitations and actions taken during capture
---
## Case Study: Silk Road
 
### Arrest & Live Evidence
- Investigators identified the suspect using **multiple evidence sources**
- Laptop was **in use at the point of arrest**
- An unlocked, running system can present **fleeting evidence**
- **Discuss:** why might shutdown or immediate seizure change what is available?
  - (Think: volatile data, RAM contents, encryption keys, open sessions)
### Imaging & Integrity
- Investigators documented and collected laptop evidence
- **Encryption** influenced acquisition strategy
- Forensic imaging and hashing supported later verification
- Read the trial testimony: distinguish **documented actions from assumptions**
- Consider what would need recording in a modern case log
### Examination Workflow
- Record **handover** from collector to examiner
- Confirm matching image hashes where supplied
- Preserve originals; examine a verified working copy
- Use appropriate tools (e.g. **FTK Imager**)
- Explain **provenance, interpretation and limitations** in reports
### Exhibits & Evidence Reviewed
| Topic | Source / notes |
|---|---|
| Payment evidence | Exhibit GX-113A: example of how a Silk Road payment was conducted |
| Linguistic evidence | Trial transcript pp. 815-818 (W2 D2). What linguistic comparison **can and cannot** demonstrate (cf. *Manhunt: Unabomber*) |
| Counterintelligence artefacts | Exhibit DX_C_le_counterintel_file: from DPR's counter-intel file on his laptop |
| Evidence lists | Ulbricht evidence lists (10 Dec 2014) |
| Scene documentation | Exhibit GX-201A: photo of the laptop taken **before touching it**, showing DPR chatting to "cirrus" |
| Examiner testimony | Trial transcript W2 D3 (22 Jan 2015): describes the process used |
 
### Undercover Operation & Arrest
- **Taking over "Cirrus":** agent Jared Der-Yeghiayan's undercover work (Ars Technica, 2015; Forbes, 2015)
- Transcript p. 820: asking DPR to log in; also exhibit GX-129C
- Arrest evidence ties the live laptop session to the suspect
---
## Key Takeaways
- Collection decisions affect what can be recovered and trusted
- Live systems create **opportunities and risks**
- Integrity hashes, logs and chain of custody support **repeatability**
- Technician and examiner responsibilities may be distinct
- Competencies 1 and 2 connect these choices to critical analysis
---
## Homework
- Review acquisition tools and their limitations
- Explore selected Silk Road exhibits and court testimony
- Watch the videos (*Manhunt: Unabomber* is optional)
- Consider how you would document a **defensible acquisition**
