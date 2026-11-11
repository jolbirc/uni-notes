# PACKERS & MACROS
22-10-26
 
---
## Contents
- Learning outcomes & resources
- Packers, stubs and UPX
- Worked example: Bline.cpp
- Dynamic analysis: Process Explorer & Process Monitor
- Office macros (VBA)
- Inspecting macros safely with oledump.py
---
## Learning Outcomes & Resources
- Explain how **packers** and **unpacking stubs** affect static analysis
- Compare file and memory observations using **DiE, Process Explorer, Process Monitor**
- Describe **VBA macros** and inspect document content **without enabling active code**
- **Resources:** *Practical Malware Analysis*; *Malware Analyst's Cookbook and DVD*
---
## Packers
- A **packer** transforms an executable and adds an **unpacking stub**
  - May use compression, encryption, protection
  - **Packing alone is not evidence of malice**
- Changes the file representation; often hides original strings/code
- Packed files are **not always smaller** (protection/encryption can increase size)
- At runtime the program or its loader reconstructs the original instructions **in memory**
- **Stub:** the code in the same program that unpacks the packed file when it runs
### UPX (Ultimate Packer for eXecutables)
- Open-source packer; compresses supported executables
- Unpack its own output with `-d`: `upx.exe -d Bline.exe -o Bline.unpacked.exe` (`-d` decompress, `-o` write to new file)
---
## Worked Example: Bline.cpp (benign)
- Source: C++ calling `system("echo demo > test.txt")` and `system("ping 127.0.0.1 -t")`
- Compiled in Visual Studio **Debug** (to make it large): **37 KB**; packed with UPX: **12 KB**
- Then treated as an unknown sample and reversed:
  1. **DiE** may identify UPX signatures, PE sections and characteristics (record what it actually reports)
  2. **PEiD** (legacy) recognises UPX but gets the **version wrong**
  3. Unpack with UPX, re-run DiE: **not packed**, sections clean
  4. PEiD after unpacking: compiler signatures become visible (still needs corroboration)
- **Why unpack?** Packed data can hide useful strings/code from static inspection. Compare packed vs unpacked with Strings; the unpacked file still needs validation
- **Where next?**
  - Static: more tools like Strings; a disassembler such as **IDA**
  - Dynamic: Process Monitor / Process Explorer; a debugger such as **Immunity**
---
## Dynamic Analysis
### Process Explorer
- Examine the process tree: parent exe, `cmd.exe` and child processes
- Colours depend on Process Explorer settings, not necessarily packing
- Compare strings in the exe **on disk** vs in the **process memory**: strings are clearer in memory (unpacked)
### Process Monitor
1. Start capture (magnifier icon), run `bline.packed.exe`, stop capture
2. Everything on the system in that window is captured, so **filter**
3. Alternative: Tools -> **Process Tree**, find the process, **Go To Event**, right-click -> **Include**
4. Review file, registry, network, process events (e.g. `cmd` processes being created)
---
## Macros
- **Macros** automate actions in Office apps, often written in **VBA**
- Macro-enabled files: Word `.docm`, Excel `.xlsm`
- Because macros execute code, examine unknown documents **without enabling them**
### Demo: NothingHereMr.docm (benign)
- Sub `AutoOpen()` that types "Material below is confidential." into the document
- Steps: new Word doc -> save as `.docm` -> Developer tab -> record/create macro (saved in the document) -> creates module `NewMacros` and sub `TestMacro` -> paste demo code -> save and close
- **Entry points:** `Sub AutoOpen()` (Word), `Sub Workbook_Open()` (Excel)
- `AutoOpen` runs only when permitted by Office policy and trust settings
- **Modern Office** typically **blocks Internet-sourced macros** by default (older versions showed "Enable Content"). Don't enable unknown macros
- **Malicious use (concept):** VBA runs additional code or triggers later stages. Compare a benign example with indicators in published malicious-document analysis; don't build or run reverse-shell macros
- **Reporting:** record what a macro **appears** to do; separate observations from assumptions
---
## Inspecting Macros Safely: oledump.py
- Offline tools inspect the Office container and recover VBA streams **without Word and without executing** anything
- **oledump.py** (Didier Stevens suite)
  1. Add the oledump directory to PATH; reopen cmd; `oledump.py -h` to test
  2. Run against the doc (e.g. `payload macro.docm`): shows macro code in `word/vbaProject.bin`
     - Lowercase **`m`** = VBA with only Attribute statements; uppercase **`M`** = more substantial VBA code
  3. Select a stream with `-s` (e.g. `-s A3`): looks like gibberish as VBA is compressed
  4. Decompress with `-v`: the readable macro code
- **VBA stomping:** stored VBA source can differ from the compiled representation, which may mislead tools that check only one. Compare multiple views and explain discrepancies
---
## Summary & Independent Study
- Packers/stubs; packing is not the same as malicious behaviour
- Used DiE, Strings, Process Explorer and Procmon on a benign example
- Recognise Word macro entry points; use oledump.py without enabling macros
- Read *Practical Malware Analysis* ch. 1-3 (tool screenshots may be dated)
- Compare packed/unpacked Bline structure; examine the supplied harmless VBA doc offline and record streams and extracted code
