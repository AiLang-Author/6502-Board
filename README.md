# 6502-Board

JSON motherboard + pluggable MOS chip models, written in [Ailang](https://github.com/AiLang-Author/Ailang-Self-Hosting-).

The **board file is a schematic**. Chips are Ailang libraries. Runtime pointers are LinkagePool; methods plug with `AddressOf` / `CallIndirect`.

```
c64.board.json                 CPU / MOS6569 / MOS6526 / MOS6581
  chips[]  ---- model ------>  Chip_Read / Write / Tick
  maps[]   ---- decode ----->  Bus6502_MapIO
  nets[]   ---- wires ------>  irq, nmi, phi2
```

Sibling of [C64-BASIC-GTK](https://github.com/AiLang-Author/C64-BASIC-GTK) (interpreter, not this machine). Canonical public repo: **https://github.com/AiLang-Author/6502-Board**

## Status

| Piece | State |
|---|---|
| NMOS 6502 documented opcodes | Klaus Dormann **PASS** (30,646,177 steps, trap `$3469`) |
| Unofficial opcodes | Lorenz CPU chain **PASS** (ANE/LXA/SHA family) |
| JSON board loader | **PASS** (`ProbeASIC` at `$DE00`) |
| C64 pack + KERNAL stubs | **PASS** (`JSR $FFD2`, VIC `$D020`, 6510 `$01`) |
| PLA `$01` fetch map | **PASS** (`mmufetch` / `mmu` / `cpuport`; `$34` is 64K RAM at `$D000`) |
| CIA | timers A+B, per-CPU-cycle phi2, start delay 3 (write+2), ICR/IMR, TA→TB cascade, PB6/PB7 overlay; IRQ pin one cycle after underflow; ICR read clears the pipe |
| IRQ / NMI | CIA1 → IRQ; CIA2 rising edge → `CPU.nmi_pending`; Lorenz `nmi` `$DD0D` t−1/t/t+1 **PASS** |
| VIC | NTSC raster `$D011`/`$D012` (65 cyc/line); paint via display libs later |
| Lorenz 2.15 | through `nmi`, CIA timers/PB/TAB, `loadth`, `cnto2`, `icr01` (warnings), `imr`, `flipos`; **TIMEOUT** on `oneshot` (`CRA IS NOT $08 AT ICR=$01`, 2e9 steps) |

## Import

```
Import.Librarys.Emulators.6502.C64
Import.Librarys.Emulators.6502.CPU
```

## Layout

```
Librarys/Emulators/6502/   CPU Bus Board C64 MOS6526 MOS6569 MOS6581
docs/emu/boards/           c64.board.json  smoke.board.json
tests/emu6502/             smoke, Klaus, Lorenz 2.15, PLA/NMI/CIA smokes
docs/emu/                  BOARD.md  6502_DESIGN.md
docs/emu/chips-ref/        floooh/chips zlib headers (reference only)
```

## Build

Needs `ailang.x` from Ailang-Self-Hosting (libraries `Arena` / `JSON` come from there). While this tree still lives inside Ailang-Self-Hosting:

```
./ailang.x tests/emu6502/nmi_smoke.ailang /tmp/nmi_smoke.x
/tmp/nmi_smoke.x
```

Standalone clone: set `AILANG` to the self-hosting tree and compile the same tests from this repo.

## License

Sean Collins Software License (SCSL). Chip reference headers under zlib (Andre Weissflog / floooh/chips). Lorenz tests public domain. Klaus Dormann functional blob as upstream license.
