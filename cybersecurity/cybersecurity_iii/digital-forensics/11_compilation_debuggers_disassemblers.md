# COMPILATION, DEBUGGERS & DISASSEMBLERS
30-10-26
 
---
## Contents
- Learning outcomes & resources
- Compilation pipeline
- File types and PE structure
- Machine code and assembly
- Debuggers
- Disassemblers and decompilers
- Reverse engineering workflow & reporting
---
## Learning Outcomes & Resources
- Explain the stages from **source -> object files -> executable**
- Recognise basic **PE format**, machine code and assembly concepts
- Distinguish **debugging, disassembly and decompilation**
- Use these concepts to interpret findings from a **Ghidra** practical
- **Resources:** Ghidra (NSA, GitHub releases); Microsoft C compiler (MSVC command-line) walkthrough; hasherezade *How to start RE/malware analysis*; *Practical Malware Analysis*
---
## Two Perspectives
- **Engineering:** start with source, compile, link, observe the program
- **Reverse engineering:** start with the binary and infer structure, functions, behaviour
- Decompilers reconstruct a useful **approximation** of source logic; they don't recover the original project
- A simple **Hello World** acts as the known reference
---
## Compilation Pipeline (C)
`hello.c` (source) -> `hello.i` (preprocessed) -> `hello.s` (assembly text) -> `hello.obj` (object code) -> `hello.exe` (PE executable)
 
**Preprocessor -> Compiler -> Assembler -> Linker**
- Simplified; toolchains may merge stages or use different intermediates
- **Example source:** `#include <stdio.h>` / `int main() { printf("Hello World!\n"); return 0; }`
- **MSVC build** (Visual Studio Developer Command Prompt):
  - `cl /c hello.c` -> object file only
  - `cl hello.c` -> compile **and link** to an exe
### File Types
| Kind | Files | Role |
|---|---|---|
| Source | `.c` / `.cpp`; `.h` | Human-readable source; declarations/interfaces |
| Object | `.obj` / `.o` | Machine code and symbols awaiting linkage; external refs may be unresolved |
| Executable | `.exe` / `.dll` | Windows **PE** files: sections, imports, loading metadata |
 
### PE Structure
- `.text`: executable instructions
- `.rdata`: read-only data; `.data`: writable initialised data
- `.rsrc`: resources (icons, dialogs, version info)
- **Imports/exports** show dependencies and interfaces; section names alone don't prove behaviour
- The Windows **loader** uses PE metadata to map and prepare a process
- Disassembly addresses/symbols differ between the object file, the exe and modern builds
---
## Machine Code, Opcodes & Assembly
- CPUs execute encoded **machine instructions**, not C source or decompiler output
- **Assembly** = human-readable mnemonics for those instructions
- x86: byte `0x90` is commonly the one-byte **NOP**
- x86/x64 instructions are **variable-length**; execution is by decoded instruction, not byte by byte
- Architecture and **calling convention** affect interpretation
---
## Debuggers
- Observe a **running process** or attached target
- **Breakpoints** and step controls inspect execution; view registers, call stacks, memory, exceptions
- Live debugging can change timing and trigger **anti-analysis** behaviour
- Debug unfamiliar code only in an isolated, authorised VM
- **Choices:**
  - **x64dbg / x32dbg:** Windows user-mode
  - **WinDbg:** strong symbol and system support
  - **GDB** (GNU/Linux), **LLDB** (LLVM)
  - Immunity Debugger, OllyDbg: legacy, in older tutorials
---
## Disassembly vs Decompilation
| | Disassembler | Decompiler |
|---|---|---|
| Output | Machine-level operations and addresses | C-like pseudocode |
| Good for | Precise instruction and control-flow reasoning | Quickly grasping likely logic |
| Caveat | Architecture-specific details matter | An **approximation**; validate before reporting |
 
- **Disassembly:** translates machine-code bytes into assembly using instruction-set knowledge, addresses and heuristics
  - Can misread embedded data, obfuscated control flow or packed content
  - Not a full reconstruction of the original source
- **Decompiler:** rebuilds loops, branches, variables, function calls
  - Original variable names, comments, types and algorithms are often missing or inferred
  - Check conclusions against disassembly, imports, strings and observed behaviour
### Tools
| Disassemble / decompile | Other approaches |
|---|---|
| **Ghidra** (open source) | **radare2** (command line) |
| **IDA** (commercial and free) | **x64dbg** (runtime debugging) |
| **Binary Ninja** (commercial) | PE inspection tools + Strings (quick triage) |
 
---
## Reverse Engineering an Unfamiliar EXE
1. Identify file type, architecture and **hashes**
2. Import into **Ghidra**; review analysis results
3. Review entry points, functions, imports, readable strings
4. Trace interesting references in **Listing** and **Decompiler** views
5. Write up what the code **appears** to do; separate evidence from inference
### Reporting a Finding
- Identify the file; record a **cryptographic hash**
- Locate the function, addresses and strings in the analysis project
- Explain behaviour in plain English with screenshots
- Identify linked/additional files; assess separately
- State **limitations:** compiled-code analysis may not show what happened at runtime
---
## Next Steps
- Ghidra practical on **instructor-approved samples** (no separate PowerPoint); IDA, Binary Ninja or other tools are allowed
- Prepares for **Assessment 1, Task 1** (malware analysis)
- **Summary:** source, object files, linker output and PE sections; when to use debugger vs disassembler vs decompiler; decompiler output is a reconstruction, not original source
- **Independent study:** complete the Ghidra read-only practical; explore hasherezade's links; review Week 4 Moodle videos; use the assessment workshop to check Task 1 progress
