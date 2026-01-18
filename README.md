# EUB-PSU “Hearth”

A **power core** for the EUB series.

- Designed for home use: prioritizes friendliness and peace-of-mind over ruggedness
- **No MCU / no firmware (Rev0)**
- Top requirement: everything should be diagnosable with only a **DMM (multimeter)**

## Goals
- Provide power for Emiuet / gum synth / chord maker, etc.
- Allow standalone bring-up and fault isolation

## Specs (Rev0 summary)
- Inputs: USB-C (fixed 5V sink) / 1S LiPo (JST-PH 2-pin) / optional DC 9V (center-negative, power only)
- PowerPath: automatic USB/battery switchover; power + charge while USB is connected
- DC 9V policy: stepped down to system rail only; must NOT feed the charger input
- Rails: SYS / +3V3 (LDO) / +5V (boost)
- External output: EUB-BUS OUT (JST XH 2.54mm 6-pin)

### EUB-BUS OUT pinout (Pin 1 → Pin 6)

| Pin | Signal |
|---:|---|
| 1 | GND |
| 2 | SYS (main system power rail) |
| 3 | +5V |
| 4 | +3V3 |
| 5 | USB_PGOOD_OD (USB input power-good, open-drain) |
| 6 | CHG_STAT_OD (charging status, open-drain) |

Notes:
- SYS is the only “system power” rail. +5V and +3V3 are always derived from SYS.
- The two status signals are open-drain outputs. Hearth does not provide pull-ups by default (pull-up footprints exist but are DNP).

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
