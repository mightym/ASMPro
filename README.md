# ASM-Pro

ASM-Pro is an assembler environment for the Amiga that runs on the Amiga
itself. The editor, macro assembler, debugger and disassembler are one program:
you write 68k code, assemble it straight into memory, run it and step through
it line by line.

It grew out of Rune Gram-Madsen's ASM-One. Solo/Genetic developed it from 1997
and released the source in 2000. Others have kept it going since, and V1.22
came out in December 2025.

This repository is a fork of [MK1Roxxor/ASMPro](https://github.com/MK1Roxxor/ASMPro)
(V1.20b). It is updated to V1.22 and fixes the crashes listed below, which only
happen on a real 68000.

## What it's good for

- **Coding demos, intros and games on the machine itself.** One command
  assembles, the next one runs.
- **Hardware-level code.** Binaries, IFF pictures and palettes can be pulled in
  at assembly time (`INCBIN`, `INCIFF`, `INCIFFP`).
- **Source-level debugging.** Single step, breakpoints, watches and conditional
  breakpoints.
- **Learning 68000 assembly.** You see the result of every instruction in the
  registers straight away.
- **Writing executables** to disk (`WO`, `WX`).

It supports every CPU from the 68000 to the 68060, FPU included.

## Requirements

- Kickstart 2.04 or newer
- `reqtools.library` v38+
- 16 KB stack

## Quick start

Esc switches between the editor and the command line.

| Command | Action |
|---|---|
| `A` | assemble the source in the editor |
| `J` | run it |
| `AD` | assemble and open the debugger |
| `X` | show the registers |
| `D` *addr* | disassemble |
| `WO` | write an executable |

In the debugger:

| Key | Action |
|---|---|
| Down | step over (runs a subroutine call as one step) |
| Right | step into |
| Esc | leave the debugger |

Start a program with `J` or `AD`. `G` continues from the current state and
does not reset the stack.

## Fixes in this fork

The authors mostly test on 68020+ machines and emulators. There, reading a
word or long at an odd address does no harm. On a plain 68000 it raises an
Address Error, and a few such spots were left.

The first four fixes were verified on an Amiga 500 (68000, no FPU,
Kickstart 3.1). The last three were checked in the code and built, but not
run on hardware.

- `AD` crashed while opening the debugger for some programs: the cursor code
  read through an uninitialised register.
- An exception before the first program run froze the whole machine. The
  exception handler restored vectors, copper lists and interrupts from
  values that had not been saved yet.
- A breakpoint at an odd address locked up the machine.
- Pointer watches at an odd address crashed the debugger.
- Stepping onto data could set a breakpoint at a random address.
- FPU instructions ran on machines without an FPU:
  - Ctrl-Shift-C in the debugger
  - `DCB`/`BLK` with a float size and no fill value
- `INCIFFP` did not check for an odd address.

Details are in [AsmPro_History.txt](AsmPro_History.txt) and the commit
messages.

## Building

ASM-Pro assembles itself on the Amiga: load `ASMPro.s` and assemble. It also
needs the AmigaOS includes. They are not in this repository, but the V1.22
source archive on Aminet ships them.

`SPEC_ED = TRUE` in `ASMPro.s` builds the special edition without the logo
graphics. The non-SE build needs the pictures from the archive's `pics/`
directory.

## Credits

- **ASM-Pro:** Solo/Genetic, based on ASM-One by Rune Gram-Madsen.
- **Later releases:** the contributors listed in
  [AsmPro_History.txt](AsmPro_History.txt).
