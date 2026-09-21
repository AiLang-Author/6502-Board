# 6502-Board

JSON motherboard + pluggable MOS chip models, written in [Ailang](https://github.com/AiLang-Author/Ailang-Self-Hosting-).

The **board file is a schematic**. Chips are Ailang libraries. Runtime pointers are LinkagePool; methods plug with `AddressOf` / `CallIndirect`.

```
c64.board.json                 Library.CPU6502 / MOS6569 / MOS6526 / MOS6581
  chips[]  ---- model ------>  Chip_Read / Write / Tick
  maps[]   ---- decode ----->  Bus6502_MapIO
  nets[]   ---- wires ------>  irq, nmi, phi2
```

Sibling of [C64-BASIC-GTK](https://github.com/AiLang-Author/C64-BASIC-GTK) (interpreter, not this machine).

## Status

| Piece | State |
|---|---|
| NMOS 6502 documented opcodes | Klaus Dormann **PASS** (30,646,177 steps, trap `$3469`) |
| Unofficial opcodes | Implemented; Lorenz smoke + chaining |
| JSON board loader | **PASS** (`ProbeASIC` at `$DE00`) |
| C64 pack + KERNAL stubs | **PASS** (`JSR $FFD2`, VIC `$D020`, 6510 `$01`) |
| Lorenz suite | **not finished** — 148 tests OK then stuck dumping `EOR (zp),Y` (`$51`) |
| VIC/SID | Register files (floooh/chips pin model next) |
| CIA | `Library.MOS6526` regs + timer A |

## Import

```
Import.Librarys.Emulators.6502.C64
Import.Librarys.Emulators.6502.CPU
```

## Layout

```
Librarys/Emulators/6502/   CPU Bus Board C64 MOS6526 MOS6569 MOS6581
boards/       c64.board.json  smoke.board.json
tests/        smoke, Klaus blob, Lorenz 2.15, board smokes
docs/         BOARD.md  6502_DESIGN.md
chips-ref/    floooh/chips zlib headers (reference only, not compiled)
```

## Build

Needs `ailang.x` from Ailang-Self-Hosting (libraries `Arena` / `JSON` come from there). From this repo:

```
export AILANG=/path/to/Ailang-Self-Hosting-
"$AILANG/ailang.x" tests/board_smoke.ailang /tmp/board_smoke.x
/tmp/board_smoke.x
```

Or keep this tree inside `Ailang-Self-Hosting-` as today (`tests/emu6502/`, `Librarys/Library.CPU6502.ailang`, `docs/emu/`).

## License

Sean Collins Software License (SCSL). Chip reference headers under zlib (Andre Weissflog / floooh/chips). Lorenz tests public domain. Klaus Dormann functional blob as upstream license.
