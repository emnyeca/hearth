# EUB-PSU “Hearth”

<p align="center">
	<img src="docs/assets/logo.png" width="420" alt="Hearth logo">
</p>

A standalone **power core and diagnostic supply** for the EUB series.

- Designed for home use: prioritizes friendliness and peace-of-mind over ruggedness
- **No MCU / no firmware (Rev0)**
- Top requirement: everything should be diagnosable with only a **DMM (multimeter)**

## Goals
- Provide an optional power source for EUB prototypes, test loads, and accessories
- Allow standalone bring-up and fault isolation

Hearth is not required for normal Emiuet operation. Emiuet retains its own
battery, charging, and system power architecture. Hearth may be used to compare
power behavior or isolate a load during development, but that test arrangement
does not define the released Emiuet architecture.

## Specs (Rev0 summary)
- Inputs: USB-C (fixed 5V sink) / 1S LiPo (JST-PH 2-pin) / optional DC 9V (center-negative, power only)
- PowerPath: automatic USB/battery switchover; power + charge while USB is connected
- DC 9V policy: stepped down to system rail only; must NOT feed the charger input
- Rails: SYS / +3V3 (LDO) / +5V (boost)
- External output: EUB-BUS OUT (JST XH 2.5mm 5-pin)

### EUB-BUS OUT pinout (Pin 1 → Pin 5)

| Pin | Signal |
|---:|---|
| 1 | GND |
| 2 | +5V (main system power, normalized 5V) |
| 3 | +3V3 (derived from +5V) |
| 4 | PWRGOOD_OD (EUB-BUS +5V valid, open-drain) |
| 5 | CHG_STAT_OD (charging status, open-drain) |

Notes:
- SYS is an internal rail name and is not exported on EUB-BUS.
- Status signals are open-drain outputs. Hearth does not provide pull-ups; the receiving device pulls up to its logic voltage.

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
