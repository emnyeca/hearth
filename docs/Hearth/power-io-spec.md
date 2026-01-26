# Hearth — Power I/O & EUB-BUS Specification (Rev0)

This document is the **user-facing and engineering-facing** specification for Hearth’s power inputs, switching rules, and EUB-BUS output.

## 1. What Hearth is
Hearth is a **power core** for the EUB series.
It is designed to validate and isolate the Emiuet power design while also serving as a reusable power + distribution unit for future EUB devices.

Rev0 has **no MCU / no firmware**.

## 2. Definitions
- **USB input (VBUS)**: 5V provided by USB-C.
- **Battery (BAT)**: 1-cell LiPo connected only to the charger.
- **System power rail (SYS)**: the main rail that represents “the system is powered”.
  - SYS is sourced either from USB/BAT (via the charger PowerPath output) or from DC 9V (via a buck converter).
- **+5V rail**: the normalized system power rail that is exported to EUB-BUS.
- **+3V3 rail**: regulated 3.3V derived from +5V.
- **EUB-BUS OUT**: Hearth’s only external output connector.

## 3. Power inputs (what the user sees)

### 3.1 USB-C IN (Power / Charge)
- Panel label: **USB-C IN (Power / Charge)**
- Function: provides system power and enables battery charging.
- USB power feeds the charger as normal.

### 3.2 DC 9V IN (Power Only) — center-negative
- Panel label: **DC 9V IN (Power Only)**
- Function: provides **system power only** (no battery charging from DC 9V).
- Intended for use when USB power is unavailable (typical guitarist pedal adapters).

Electrical policy (must hold true):
- DC 9V must **not** connect to the charger input.
- DC 9V is stepped down to the system rail (SYS-equivalent).
- Battery charging remains exclusively handled by **USB via the charger**.

## 4. Power path & switching (multiple inputs → normalized +5V)
Hearth may have multiple power inputs (USB / BAT via charger PowerPath, and optional DC 9V).
Regardless of the active input, Hearth must provide a **normalized +5V rail** to EUB-BUS.

Rules:
- USB / BAT / DC 9V may be connected in any combination.
- Input priority and reverse-current blocking must be solved in hardware (ideal-diode / power-mux style).

Implementation intent:
- Use the DC jack’s detect contact **only as a control signal** (if used).
- Do **not** route system current through mechanical jack contacts.
- Prevent back-feeding between sources and prevent back-powering external equipment.

## 5. Battery policy (isolation)
- The battery connects **only** to the charger.
- The battery must not have any alternate path to SYS or EUB-BUS.
- When the charger system output is disconnected, the battery must be **fully isolated** from system power.

## 6. EUB-BUS OUT (fixed connector spec)

### 6.1 Connector
- Type: **JST XH series**
- Pitch: **2.5mm**
- Pins: **5**
- Board-side: **vertical header**
- Cable-side: standard XH housing

Pin 1 must be **GND** (square pad / silkscreen indicator).

### 6.2 Pin order (Pin 1 → Pin 5)
Pin numbering follows standard JST XH convention (Pin 1 marked by square pad / silkscreen indicator).

| Pin | Name | Description |
|---:|---|---|
| 1 | GND | Ground |
| 2 | +5V | Main system power (normalized 5V, regardless of USB / DC 9V / BAT source) |
| 3 | +3V3 | Regulated 3.3V derived from +5V |
| 4 | PWRGOOD_OD | System power-good status (open-drain output) |
| 5 | CHG_STAT_OD | Battery charging status (open-drain output) |

### 6.3 Electrical notes
- SYS is an internal rail name and must not be exported on EUB-BUS.
- +5V is the only exported “power rail contract” on EUB-BUS; +3V3 is derived from +5V.
- PWRGOOD_OD indicates “**EUB-BUS +5V is valid (within spec)**”, regardless of which input source is active.
- Hearth must not export any USB-only PGOOD signal to EUB-BUS.

## 7. PWRGOOD / CHG_STAT signals (open-drain, no pull-ups)
- PWRGOOD_OD and CHG_STAT_OD are exported to EUB-BUS as **open-drain** signals.
- Hearth must **not** pull these up (no onboard pull-ups).
- The receiving device decides the logic voltage (e.g. pull up to +3V3 or +5V).

Rationale:
- The receiving device (e.g. Emiuet) decides the pull-up voltage and timing.
- Avoids unexpected back-powering or “phantom powering” through status lines.
