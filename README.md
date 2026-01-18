# EUB-PSU “Hearth”

A **power core** for the EUB series.

- Designed for home use: prioritizes friendliness and peace-of-mind over ruggedness
- **No MCU / no firmware (Rev0)**
- Top requirement: everything should be diagnosable with only a **DMM (multimeter)**

## Goals
- Provide power for Emiuet / gum synth / chord maker, etc.
- Allow standalone bring-up and fault isolation

## Specs (Rev0 summary)
- Inputs: USB-C (fixed 5V sink) / 1S LiPo (JST-PH 2-pin)
- PowerPath: automatic USB/battery switchover; power + charge while USB is connected
- Rails: SYS / +3V3 (LDO) / +5V (boost)
- External output: EUB-BUS OUT (JST XH 2.54mm 6-pin)

## Documents
- Design intent / constraints: docs/design.md
- Power notes: docs/power_notes.md
- Mechanical notes: docs/mech_notes.md
- Operations: docs/operations.md

## KiCad
- Requires KiCad 9.x
- KiCad sources live in kicad/

## Repo conventions
- The Rev is tracked at the top of docs/design.md
- Manufacturing outputs (Gerber/BOM, etc.) are exported to fab/ (see docs/operations.md)
