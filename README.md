# ASM-Pro

ASM-Pro is a complete, professional 68k development environment that runs on
the Amiga itself. The editor, macro assembler, optimizer, source-level
debugger, monitor and disassembler are one program. Code is assembled
straight into memory, runs a keystroke later, and can be stepped through line
by line in the source you just wrote.

It comes from the ASM-One family, the tools that a large part of the Amiga
scene wrote its demos, intros and games in. ASM-One is by Rune Gram-Madsen.
Solo/Genetic started ASM-Pro in 1997 and released its source in 2000. Others
have kept it going since, and V1.22 came out in December 2025.

This repository is a fork of [MK1Roxxor/ASMPro](https://github.com/MK1Roxxor/ASMPro)
(V1.20b). It is updated to V1.22 and fixes the crashes listed below, which only
happen on a real 68000.

## What it's good for

- **Serious assembly development on the machine itself.** You get:
  - an editor with syntax colouring and ten source buffers
  - a fast assembler, fast enough to assemble ASM-Pro's own 55,000-line
    source
  - a debugger, a monitor and a disassembler

  There is no toolchain to set up and no round trip through a
  cross-assembler.
- **Every CPU from the 68000 to the 68060, FPU included.** It also covers the
  Apollo 68080. One tool covers the stock A500 and the accelerated big-box
  Amiga alike.
- **A real macro assembler:**
  - macros and conditional assembly
  - `REPT`/`QREPT` repeat blocks
  - sections for chip and fast memory
  - includes and `INCBIN`
  - IFF pictures and palettes converted at assembly time (`INCIFF`,
    `INCIFFP`)
  - an optimizer

  Output is an executable, a linkable object or a raw binary.
- **A proper debugger:**
  - step into and step over
  - run to cursor
  - breakpoints with conditions
  - watches, animate mode and register editing

  When your code crashes, ASM-Pro reports the exception with the faulting
  instruction instead of taking the machine down.
- **Hardware at your fingertips:**
  - a built-in reference of the custom chip registers
  - bootblock checksum and bootblock simulator
  - raw sector and track access
  - a sine table generator for your effects
- **Monitor and disassembler.** Edit memory; dump it as hex, ASCII or
  binary; search, compare, copy and fill it. You can also disassemble any
  piece of memory straight into your source.
- **Doing crazy stuff like Scoopex.** Copper bars in every colour of the
  rainbow, sine scrollers and more bobs than 512 KB of chip RAM should allow.
  Scoopex have been doing this sort of thing on the Amiga for decades, and the
  GitHub repository this fork comes from was put up by StingRay/Scarab^Scoopex.
  Results may vary. Greetings are mandatory.

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

In the debugger, Down steps over, Right steps into and Esc leaves.

Start a program with `J` or `AD`. `G` continues from the current state and
does not reset the stack.

## Command reference

Commands are typed at the command line. Case doesn't matter. Commands that
work on memory ask for the addresses (`BEG>`, `END>`, `DEST>`) when you don't
give them.

### Assembling and running

| Command | Action |
|---|---|
| `A` | assemble |
| `AO` | assemble with optimizations |
| `AC` | check the source only, no code is generated |
| `AD` | assemble and start the debugger |
| `AS`*n* | switch to source buffer *n* (0–9) |
| `J` [*addr*] | run the program, or jump to *addr* |
| `G` [*addr*] | go: continue, asking for breakpoints first |
| `K` [*n*] | single step *n* instructions |
| `X` | show the registers |
| `ZB` | clear all breakpoints |
| `PS` | set the parameters passed to the program |
| `=` | object info: the sections of the assembled program |
| `=S` | show the symbol table |
| `E` [*n*] | load the files named in `EXTERN` directives |

### Files

| Command | Action |
|---|---|
| `R` [*file*] | read a source |
| `R0`–`R9` | read one of the recent sources |
| `RN` | read a text file, with no source name pattern |
| `RB` | read a binary into memory |
| `RO` | read an object (executable) into memory |
| `RE` | read an environment (project) |
| `W` [*file*] | write the source |
| `WN` | write as text |
| `WB` | write memory as a binary |
| `WO` | write an executable |
| `WX` | write an executable with short code and data hunks |
| `WL` | write a linkable object |
| `WE` | write the environment (project) |
| `WP` | write the preferences |
| `U` | update: write the source back to its file |
| `UA` | update all sources of the project |
| `I` | insert a file into the source |
| `V` [*dir*] | show a directory |
| `CD` | create a directory |
| `ZF` | delete a file |
| `!`*pattern* | set the source file name pattern, e.g. `!(.s\|.asm)` |

### Source

| Command | Action |
|---|---|
| `T` [*n*] | go to the top, or to line *n* |
| `B` | go to the bottom |
| `L` | search the source |
| `P` | print lines |
| `ZL` | delete lines |
| `ZS` | zap (clear) the source |
| `O` | old: bring back the zapped source |
| `EL` | extend labels with a prefix or suffix |
| `=P` | project info |

### Memory

| Command | Action |
|---|---|
| `M` [*addr*] | edit memory in the monitor |
| `D` [*addr*] | disassemble |
| `H` [*addr*] | hex dump |
| `N` [*addr*] | ASCII dump |
| `BM` | binary dump |
| `@D` `@H` `@N` `@B` | one line of disassembly, hex, ASCII or binary |
| `@A` | assemble single instructions into memory |
| `S` | search memory |
| `F` | fill memory |
| `C` | copy memory |
| `Q` | compare memory |
| `ID` `IH` `IN` `IB` | insert a disassembly, hex, ASCII or binary dump of memory into the source |
| `CS` / `IS` | create a sine table in memory / in the source |
| `ZA` | free the memory the assembler allocated for sections |
| `ZI` | free the cached include files |
| `=M` | add work memory |
| `=A` | show where ASM-Pro itself sits in memory |

### Disk and bootblock

| Command | Action |
|---|---|
| `RS` / `WS` | read / write a sector |
| `RT` / `WT` | read / write a track |
| `CC` | calculate a bootblock checksum |
| `BS` | bootblock simulator: run a bootblock from memory |

### Other

| Command | Action |
|---|---|
| `?`*expr* | calculator |
| `[`*expr* | floating point calculator (needs an FPU) |
| `=R` | custom chip register reference |
| `=C` | colours |
| `Y` *command* | run an AmigaDOS command |
| `>` | send the output of commands to a file |
| `#` | about |
| `!` | quit (asks first) |
| `!!` | quit at once |
| `!R` | restart |

### Editor keys

Amiga means the right Amiga key. An upper-case letter means Amiga + Shift.

| Key | Action |
|---|---|
| Esc | back to the command line |
| F1–F10 | switch source buffer |
| Help | ASM-Pro help (AmigaGuide) |
| Amiga+`b` | mark block |
| Amiga+`c` / `x` | copy / cut block |
| Amiga+`v` or `i` | insert block |
| Amiga+`u` | unmark |
| Amiga+`q` | select all |
| Amiga+`;` / `:` | comment / uncomment block |
| Amiga+`l` / `L` | block to lower / upper case |
| Amiga+`p` | tabulate block |
| Amiga+`K` | spaces to tabs |
| Amiga+`f` / `n` | fill / vertical fill |
| Amiga+`y` | rotate block |
| Amiga+`k` | register |
| Amiga+`W` | write block to a file |
| Amiga+`S` / `s` | search / search forward |
| Amiga+`R` / `r` | replace / replace forward |
| Amiga+`d` | delete line |
| Amiga+`j` | jump to line |
| Amiga+`J` | jump to the next `;;` mark |
| Amiga+`e` | jump to the next error |
| Amiga+`t` / `T` | top / bottom |
| Amiga+`a` / `z` | 100 lines up / down |
| Shift+Left / Right | start / end of line |
| Shift+Up / Down | page up / down |
| Alt+Left / Right | word left / right |
| Amiga+`!` `@` `#` … `)` | set mark 1–10 (the shifted number keys) |
| Amiga+`1`…`0` | jump to mark 1–10 |
| Amiga+`,` / `m` | record / play a macro |
| Amiga+`g` | grab the word under the cursor |
| Amiga+`h` | number to ASCII |
| Amiga+`A` | assemble |
| Amiga+`O` | optimize |
| Amiga+`D` | debugger |
| Amiga+`M` | monitor |
| Amiga+`[` / `]` | environment / assembler preferences |
| Amiga+`Z` | syntax colours |

### Debugger keys

| Key | Action |
|---|---|
| Down | step one instruction, over subroutine calls |
| Right | step into |
| Amiga+`r` | run |
| Amiga+`s` | step *n* instructions |
| Amiga+`u` | run until the cursor line |
| Amiga+`k` | skip the instruction |
| Amiga+`i` | animate |
| Amiga+`x` | edit the registers |
| Amiga+`m` | edit memory |
| Amiga+`b` / `B` | breakpoint at the mark / at an address |
| Amiga+`f` | breakpoint condition |
| Amiga+`z` | clear all breakpoints |
| Amiga+`G` | clear the conditional breakpoints |
| Amiga+`a` | add a watch |
| Amiga+`1`…`8` | delete watch 1–8 |
| Amiga+`Z` | clear all watches |
| Amiga+`j` / `J` | jump to the mark / to an address |
| Amiga+`C` | switch between Dx and FPx registers (FPU only) |
| Esc | leave the debugger |

### Monitor keys

| Key | Action |
|---|---|
| Amiga+`d` `h` `n` `b` | disassembly, hex, ASCII or binary view |
| Amiga+`j` | jump to an address |
| Amiga+`l` | back to the last address |
| Amiga+`q` | quick jump |
| Amiga+`!` `@` `#` / `1` `2` `3` | set / jump to mark 1–3 |
| Amiga+`,` / `.` | set start / end |
| Amiga+`w` | save the range as a binary |
| Amiga+`s` / `f` | search / search forward |
| Esc | leave the monitor |

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
