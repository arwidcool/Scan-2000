# SCAN2000 — 10-Channel Replacement Scanner Cards for Keithley 2000 & 2001

> **Open-source hardware** replacement scanner cards for the Keithley 2000 and 2001 Digital Multimeters.  
> Designed by **Arwid Vasilev** · PCB Outline by **Patrick Baus**  
> Designed with [EasyEDA Pro](https://pro.easyeda.com/) · Version 1.0 · March 2026

---

## Table of Contents

- [Overview](#overview)
- [Variants](#variants)
  - [2000-SCAN Omron (Standard)](#2000-scan-omron-standard)
  - [2000-SCAN Panasonic (Standard)](#2000-scan-panasonic-standard)
  - [2000-SCAN Panasonic DB25 (Test Fixture)](#2000-scan-panasonic-db25-test-fixture)
  - [2001-SCAN Omron](#2001-scan-omron)
- [Electrical Specifications](#electrical-specifications)
- [How It Works — Circuit Architecture](#how-it-works--circuit-architecture)
- [Key Components](#key-components)
- [Repository Structure](#repository-structure)
- [Manufacturing Files](#manufacturing-files)
- [Safety & Warnings](#safety--warnings)
- [Credits](#credits)

---

## Overview

The Keithley 2000 and 2001 bench multimeters accept optional scanner cards (2000-SCAN / 2001-SCAN) that allow the instrument to automatically switch between up to 10 measurement channels. Original Keithley scanner cards are expensive, obsolete, or have aging relay contacts that affect measurement accuracy.

This project provides **drop-in replacement PCBs** that plug directly into the existing card slot of the Keithley 2000 or 2001 using the original card-edge connector. They implement the same 10-channel switching matrix using modern, readily available relays and shift-register control logic, bringing the cost down substantially while maintaining full functional and electrical compatibility.

There are **four variants** covering different relay types, target instruments, and output connector styles.

---

## Variants

### 2000-SCAN Omron (Standard)

| Property | Value |
|---|---|
| Folder | `2000 Omron/` |
| Schematic | Created 2026-03-19 · Updated 2026-03-20 |
| Target Instrument | Keithley 2000 |
| Relay Type | Omron — **CA6V-2-D5V** (CH1–CH5 rows), **G6BK-2-D2V** (CH6–CH10 / OUT) |
| Output Connector | 2× 12-position screw terminal blocks (5.08 mm pitch) |
| Max Voltage | **200 VDC / 125 VAC** |
| Max Current | **1 A** |
| Measurement Types | **Voltage & Resistance ONLY** |
| Earth Isolation | **220 V to Earth maximum** |

This is the standard 10-channel card for the Keithley 2000 using Omron through-hole reed relays. The mixed relay population (CA6V on the upper row, G6BK on the lower/output row) matches the channel architecture of the original Keithley design. Two green 12-position screw terminal strips on the top edge provide H/L connections for CH1–CH5 + OUT A and CH6–CH10 + OUT B respectively.

---

### 2000-SCAN Panasonic (Standard)

| Property | Value |
|---|---|
| Folder | `2000 Panasonic/` |
| Schematic | Created 2026-03-20 · Updated 2026-03-22 |
| Target Instrument | Keithley 2000 |
| Relay Type | Panasonic — **TQ2-L2-5V** magnetic latching relay (×11 total) |
| Output Connector | 2× 12-position screw terminal blocks (5.08 mm pitch) |
| Max Voltage | **200 VDC / 125 VAC** |
| Max Current | **1 A** |
| Measurement Types | **Voltage & Resistance ONLY** |
| Earth Isolation | **220 V to Earth maximum** |

Identical layout to the Omron variant but uses Panasonic TQ2-L2-5V **magnetic latching relays** throughout. Magnetic latching relays hold their state without continuous coil current and release only when driven in reverse, reducing power consumption and heat. A **390 mΩ** resistor (R5, 1206) is added to the supervisor timing circuit to compensate for the slightly different coil characteristics of the Panasonic relay (original RC = ~1.3 s; resistor trims compatibility with Keithley's timing logic).

---

### 2000-SCAN Panasonic DB25 (Test Fixture)

| Property | Value |
|---|---|
| Folder | `2000 Panasonic DB25/` |
| Schematic | Created 2026-03-20 · Updated 2026-03-21 |
| Target Instrument | Keithley 2000 |
| Relay Type | Panasonic — **TQ2-L2-5V** magnetic latching relay (×11 total) |
| Output Connector | DB25 female (D-Sub 25) + 3-pin screw terminal for OUT B/H/L |
| Max Voltage | **90 VDC** (low-voltage variant) |
| Measurement Types | **Voltage & Resistance ONLY** |
| Special Feature | **DB25 Test Fixture Scanner Card** |

This variant routes all 10 channel H/L lines to a **DB-25 female connector**, making it ideal for use with custom test fixtures where you need the scanner to automatically probe a DUT connected via a 25-pin cable. OUT B remains accessible via a small 3-pin screw terminal. The voltage rating is reduced to 90 V maximum because of the lower isolation margin of the DB25 connector versus the high-voltage screw terminals.

---

### 2001-SCAN Omron

| Property | Value |
|---|---|
| Folder | `2001 Omron/` |
| Schematic | Created 2026-03-17 · Updated 2026-03-20 |
| Target Instrument | Keithley 2001 |
| Relay Type | Omron — **G6SK-2-DC5V** signal relay (×11 total) |
| Output Connector | 2× 12-position screw terminal blocks (5.08 mm pitch) |
| CH 1–4 Max Voltage | **200 VDC / 125 VAC / 1 A** |
| CH 5–9 Earth Isolation | **220 V to Earth maximum** |
| CH 5 + CH 10 Max Current | **150 mA** |
| Measurement Types | **Voltage & Resistance ONLY** |
| Added Protection | Gas Discharge Tubes + Solid-State Relay protection on high-voltage channels |

The 2001 variant targets the Keithley 2001 which has a different internal bus architecture. The card features **enhanced overvoltage protection**:

- **8× SMD2921-300N** Gas Discharge Tube (GDT) transient suppressors on the measurement lines
- **2× GAQW210E** Solid-State Relays (MOS output, DIP-8) for isolation on the sensitive high-frequency channels
- **100 Ω** current-limiting resistors (R1210 package) on signal lines entering the relay matrix

The split channel rating (CH 1–4 full voltage vs. CH 5–10 limited current) reflects the 2001's internal front-end architecture where some channels are connected to higher-impedance inputs.

---

## Electrical Specifications

| Spec | 2000 Omron | 2000 Panasonic | 2000 Panasonic DB25 | 2001 Omron |
|---|---|---|---|---|
| Max DC Voltage | 200 V | 200 V | 90 V | 200 V (CH1–4) |
| Max AC Voltage | 125 V | 125 V | 90 V | 125 V (CH1–4) |
| Max Current | 1 A | 1 A | N/A | 1 A (CH1–4) / 150 mA (CH5, CH10) |
| Earth Isolation | 220 V | 220 V | N/A | 220 V |
| Relay Supply | 5 V | 5 V | 5 V | 5 V |
| Board Supply | From Keithley card slot | From Keithley card slot | From Keithley card slot | From Keithley card slot |
| Channels | 10 + 2 outputs (OUT A, OUT B) | 10 + 2 outputs | 10 + 2 outputs | 10 + 2 outputs |

---

## How It Works — Circuit Architecture

All four variants share the same core control architecture derived from the Keithley 2000/2001 scanner card slot interface.

```
Keithley Instrument Card Slot
  │
  ├─ DATA  ────────────────────────────────────────┐
  ├─ SRC_CLK (CLK) ────────────────────────────────┤
  ├─ STROBE ───────────────────────────────────────┤  Serial shift register chain
  ├─ RST_OUT (NRST) ───────────────────────────────┤
  └─ +5V / GND ─────────────────────────────────── Power
                                                   │
              ┌────────────────────────────────────┤
              │                                    │
         ┌────▼────┐   ┌─────────┐   ┌─────────┐  │
         │TPIC6B595│──▶│TPIC6B595│──▶│TPIC6B595│  │
         │  U1/U6  │   │  U2/U7  │   │  U3/U8  │  │
         │ (8 bits)│   │ (8 bits)│   │ (8 bits)│  │
         └────┬────┘   └────┬────┘   └────┬────┘  │
              │              │              │       │
         CH1-8 relay coils  CH9-10 + OUT A,B coils  │
                                                    │
              ┌─────────────────────────────────────┘
              │
         ┌────▼─────────┐
         │ TPS3808G01   │  Voltage Supervisor
         │ (SOT-23-6)   │  — Monitors VCC via resistor divider
         │              │  — Generates RESET on power loss
         │  200 µF cap  │  — ~1.25 s delay capacitor
         └──────────────┘  — Clears shift registers on reset
```

### Shift Register Chain (TPIC6B595DWR)

Three **Texas Instruments TPIC6B595** 8-bit power logic shift registers are daisy-chained in series. Each TPIC6B595 is an SOIC-20 device with open-drain DMOS outputs rated for up to 50 V / 150 mA per output — sufficient to directly drive relay coils without additional drivers. The 24 outputs (3 × 8) control the 10 relay channels (each relay having 2 coils for set/reset on latching types, or single coil on non-latching types) plus the two output switch relays.

The serial interface uses:
- **DATA** — Serial data input from the Keithley
- **SRC_CLK** — Shift clock
- **STROBE** — Latch / output enable (also routed via RST_OUT for synchronised clear)
- **NRST** — Active-low reset controlled by the supervisor; clears data stuck in the shift register chain on power cycling or instrument reset

### Voltage Supervisor (TPS3808G01DBVR)

The **TI TPS3808G01** in SOT-23-6 acts as a brown-out / reset supervisor.  ** 4.5 V** of the supply rail. The RESET output is used to:
1. Pull NRST low, clearing the shift registers and de-energising all relays on power loss
2. Prevent relay chatter during slow power-up ramps

A **220nF MLCC** on the timing pin creates approximately a **1.25-second power-on delay** before the card becomes active — matching the Keithley 2000's internal initialisation timing (original RC time ≈ 1.3 s).

For the Panasonic variant, a **390 mΩ** resistor (R5) is inserted in the supervisor's timing circuit to adjust the time constant for compatibility with the Panasonic relay coil impedance characteristics.

### Decoupling & Power Filtering

- **4× 100 nF** MLCC capacitors (C0603) — local VCC decoupling at each TPIC6B595
- **4× 22 µF** MLCC capacitors (C0805) — bulk decoupling on 5 V supply rail

---

## Key Components

### Shared Across All Variants

| Reference | Part | Package | Description |
|---|---|---|---|
| U1–U3 (or U6–U8) | **TPIC6B595DWR** | SOIC-20 | 8-bit power logic shift register, open-drain outputs, TI |
| U4 (or U13) | **TPS3808G01DBVR** | SOT-23-6 | Voltage supervisor with programmable delay, TI |
| CN1 | **3511232BFRS0BNA1** | Through-hole | Keithley card-slot edge connector (JILN) |
| CN2, CN3 | **DB127S-5.08-12P-GN-S** | Through-hole | 12-position 5.08 mm screw terminal block (DORABO) |
| C1–C4 | **100 nF** | C0603 | MLCC decoupling, Samsung |
| C5/C6 | **220 nF** | C0603 | MLCC supervisory decoupling, Samsung |

### 2000 Panasonic Specific

| Reference | Part | Package | Description |
|---|---|---|---|
| K1–K11 | **Panasonic TQ2-L2-5V** | Through-hole relay | 5V magnetic latching DPDT signal relay |
| C7–C10 | **22 µF** | C0805 | MLCC bulk supply decoupling, Samsung |
| R5 | **390 mΩ** | R1206 | Ressitor to emulate ESR of original Tantalum capacitor |

### 2001 Omron Specific

| Reference | Part | Description |
|---|---|---|
| RELAY1–RELAY11 | **Omron G6SK-2-DC5V** | 5V SPDT signal relay, 0.01 A / 1.0 A rating |
| U1–U3, U5, U9, U11, U12, U14 | **SMD2921-300N** | Gas Discharge Tube (GDT) transient suppressor, 300 V, SMD |
| U4, U10 | **GAQW210E** | Solid-State Relay (MOS output), DIP-8 |
| R1, R2, R3, R5, R7, R10, R11, R13 | **100 Ω** | R1210 current limiting on signal lines |
| C6 | **100 µF** | Aluminium electrolytic through-hole, 8 mm diameter |

---

## Repository Structure

```
Scan 2000/
├── README.md
│
├── 2000 Omron/                          # Keithley 2000 — Omron relay variant
│   ├── Keithley 2000 Omron SCAN V1.0.epro          # EasyEDA Pro project file
│   ├── Gerber_Keithley_2000_Omron_SCAN_V1.0_*.zip  # Fabrication Gerbers
│   ├── PickAndPlace_Keithley 2000 Omron SCAN V1.0_*.xlsx
│   ├── SCH_Keithley 2000 Omron SCAN V1.0_1-Main_*.png  # Schematic image
│   ├── SCH_Keithley 2000 Omron SCAN V1.0_*.pdf         # Schematic PDF
│   ├── 3D_Keithley 2000 Omron SCAN V1.0_*.png          # 3D top render
│   └── 3D bot _Keithley 2000 Omron SCAN V1.0_*.png     # 3D bottom render
│
├── 2000 Panasonic/                      # Keithley 2000 — Panasonic latching relay variant
│   ├── Keithley 2000 SCAN Panasonic V1.0.epro
│   ├── Gerber_Keithley_2000_SCAN_Panasonic_V1.0_*.zip
│   ├── BOM_Keithley 2000 SCAN Panasonic V1.0_*.xlsx
│   ├── PickAndPlace_Keithley 2000 SCAN Panasonic V1.0_*.xlsx
│   ├── SCH_Keithley 2000 SCAN Panasonic V1.0_1-Main_*.png
│   ├── SCH_Keithley 2000 SCAN Panasonic V1.0_*.pdf
│   ├── 3D_Keithley 2000 SCAN Panasonic V1.0_*.png
│   └── 3D Bot_Keithley 2000 SCAN Panasonic V1.0_*.png
│
├── 2000 Panasonic DB25/                 # Keithley 2000 — DB25 test fixture variant
│   ├── Keithley 2000 SCAN Panasonic DB25 V1.0.epro
│   ├── Gerber_Keithley_2000_SCAN_Panasonic_DB25_V1.0_*.zip
│   ├── BOM_Keithley 2000 SCAN Panasonic DB25 V1.0_*.xlsx
│   ├── PickAndPlace_Keithley 2000 SCAN Panasonic DB25 V1.0_*.xlsx
│   ├── SCH_Keithley 2000 SCAN Panasonic DB25 V1.0_1-Main_*.png
│   ├── SCH_Keithley 2000 SCAN Panasonic DB25 V1.0_*.pdf
│   ├── 3D_Keithley 2000 SCAN Panasonic DB25 V1.0_*.png
│   └── 3D Bot_Keithley 2000 SCAN Panasonic DB25 V1.0_*.png
│
└── 2001 Omron/                          # Keithley 2001 — Omron relay variant
    ├── BOM_Keithley 2001 Omron SCAN V1.0_*.xlsx
    ├── Gerber_Keithley_2001_Omron_SCAN_V1.0_*.zip
    ├── PickAndPlace_Keithley 2001 Omron SCAN V1.0_*.xlsx
    ├── SCH_Keithley 2001 Omron SCAN V1.0_1-Main_*.png
    ├── SCH_Keithley 2001 Omron SCAN V1.0_*.pdf
    ├── 3D_Keithley 2001 Omron SCAN V1.0_*.png
    └── 3D bot_Keithley 2001 Omron SCAN V1.0_*.png
```

---

## Manufacturing Files

Each variant folder contains a complete set of production-ready files:

| File Type | Description |
|---|---|
| `Gerber_*.zip` | Industry-standard Gerber files (RS-274X) + drill files. Send directly to JLCPCB, PCBWay, OSHPark, etc. |
| `BOM_*.xlsx` | Bill of Materials with LCSC part numbers, manufacturer part numbers, quantities, JLCPCB stock levels, and pricing. Ready for JLCPCB SMT assembly service upload. |
| `PickAndPlace_*.xlsx` | Centroid / pick-and-place file for SMT assembly. Compatible with JLCPCB assembly service. |
| `SCH_*.pdf` | Full-resolution schematic PDF for reference. |
| `SCH_*_Main_*.png` | Schematic screenshot for quick reference. |
| `3D_*.png` | 3D rendered image of the top side of the PCB. |
| `3D bot_*.png` | 3D rendered image of the bottom side of the PCB. |
| `*.epro` | EasyEDA Pro native project file. Open in [EasyEDA Pro](https://pro.easyeda.com/) to modify the schematic or PCB. |

### Ordering at JLCPCB

1. Download the `Gerber_*.zip` file for your chosen variant.
2. Upload to [jlcpcb.com](https://jlcpcb.com) — the board dimensions and layers are pre-configured.
3. For assembled boards: also upload `BOM_*.xlsx` and `PickAndPlace_*.xlsx` in the JLCPCB SMT assembly section. All SMD parts have LCSC part numbers pre-filled.
4. Through-hole relays (all variants) and the card-edge connector (CN1) require hand soldering — they are not included in the JLCPCB SMT assembly BOM.

---

## Safety & Warnings

> **READ BEFORE USE**

### General
- These cards are designed for **voltage and resistance measurement ONLY**. They are **not rated for current measurement** routing through the switch matrix (except via the OUT channels at specified limits).
- Maximum switched voltage:
  - **200 VDC / 125 VAC** for 2000 Omron and 2000 Panasonic variants
  - **90 VDC** for 2000 Panasonic DB25 variant
  - **200 VDC / 125 VAC on CH 1–4** for 2001 Omron; **CH 5–10 limited** (see specs)
- **220 V maximum from any pin to Earth/Chassis ground** on standard variants.
- Maximum switched current: **1 A** on CH 1–10 (standard variants); **150 mA** on CH 5 and CH 10 of the 2001 variant.

### Installation
- **Power off the Keithley instrument completely** before inserting or removing scanner cards.
- Ensure you select the correct variant for your instrument (2000 vs. 2001 — the card bus pinouts differ between instruments).
- The card-edge connector is keyed — do not force the card if resistanace is felt.
- Always verify the correct scanner card type is selected in the Keithley's front-panel setup menu after installation.

### DB25 Test Fixture Variant
- The DB25 variant is intended for **low-voltage test fixtures only (MAX 90 V)**.
- Do not connect mains-referenced signals to the DB25 variant.
- Ensure adequate isolation in your test fixture wiring.

---

## Credits

| Role | Name |
|---|---|
| Circuit & PCB Design | **Arwid Vasilev** — [github.com/arwidcool](https://github.com/arwidcool) |
| PCB Mechanical Outline | **Patrick Baus** |
| EDA Tool | [EasyEDA Pro](https://pro.easyeda.com/) |

---

## License

This project is open-source hardware. See the repository license file for details.

---

*Version 1.0 — March 2026*
