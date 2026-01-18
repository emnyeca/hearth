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

## 4. Power path & switching (mutual exclusion)
The system power sources are **mutually exclusive**:
- Source A: USB/BAT system output (charger PowerPath output)
- Source B: DC 9V buck output

Rules:
- When **DC 9V is present**:
  - System power (SYS) is supplied from DC 9V (via buck converter).
  - The charger system output is **electrically disconnected** from SYS.
  - USB may still be connected to charge the battery simultaneously.

Implementation intent:
- Use the DC jack’s detect contact **only as a control signal**.
- Do **not** route system current through mechanical jack contacts.
- Perform actual power isolation using FET / ideal-diode style power switching.

## 5. Battery policy (isolation)
- The battery connects **only** to the charger.
- The battery must not have any alternate path to SYS or EUB-BUS.
- When the charger system output is disconnected, the battery must be **fully isolated** from system power.

## 6. EUB-BUS OUT (fixed connector spec)

### 6.1 Connector
- Type: **JST XH series**
- Pitch: **2.54mm**
- Pins: **6**
- Board-side: **vertical header**
- Cable-side: standard XH housing

### 6.2 Pin order (Pin 1 → Pin 6)
Pin numbering follows standard JST XH convention (Pin 1 marked by square pad / silkscreen indicator).

| Pin | Name | Description |
|---:|---|---|
| 1 | GND | Ground |
| 2 | SYS | Main system power rail (only rail that represents “system power”) |
| 3 | +5V | Regulated 5V derived from SYS |
| 4 | +3V3 | Regulated 3.3V derived from SYS |
| 5 | USB_PGOOD_OD | USB input-good status, open-drain output |
| 6 | CHG_STAT_OD | Battery charging status, open-drain output |

### 6.3 Electrical notes
- +5V and +3V3 are always derived from SYS.
- USB_PGOOD_OD indicates **USB input good**, not “system power good”.

## 7. PGOOD / CHG signals (open-drain, no default pull-ups)
- USB_PGOOD_OD and CHG_STAT_OD are exported as **open-drain** signals.
- Hearth must **not** pull these up by default.
- Pull-up resistors (e.g. 10k to +3V3) may exist as footprints only.
  - Default assembly state: **DNP (Do Not Populate)**.

Rationale:
- The receiving device (e.g. Emiuet) decides the pull-up voltage and timing.
- Avoids unexpected back-powering or “phantom powering” through status lines.
