# SCAN2000 - 10-Channel Replacement Scanner Cards for Keithley 2000 & 2001

Open-source hardware replacement scanner cards for the Keithley 2000 and 2001 Digital Multimeters.
Designed by **Arwid Vasilev** · PCB Outline by **Patrick Baus** · V1.0 · March 2026

---

## Overview

Drop-in replacement PCBs for the Keithley 2000-SCAN and 2001-SCAN scanner cards. They plug directly into the existing card slot using the original card-edge connector and provide the same 10-channel switching matrix using modern relays and shift-register control logic.

Four variants are available covering different relay types, instruments, and connector styles.

---

## Variants

### 2000-SCAN Omron

![2000 Omron Top](2000%20Omron/3D_Keithley%202000%20Omron%20SCAN%20V1.0_2026-03-22.png)
![2000 Omron Bottom](2000%20Omron/3D%20bot%20_Keithley%202000%20Omron%20SCAN%20V1.0_2026-03-22.png)

| Property | Value |
|---|---|
| Target Instrument | Keithley 2000 |
| Relay | Omron CA6V-2-D5V / G6BK-2-D2V |
| Output Connector | 2x 12-position screw terminals (5.08 mm) |
| Max Voltage | 200 VDC / 125 VAC |
| Max Current | 1 A |
| Measurement | Voltage & Resistance ONLY |
| Earth Isolation | 350 V peak |

---

### 2000-SCAN Panasonic

![2000 Panasonic Top](2000%20Panasonic/3D_Keithley%202000%20SCAN%20Panasonic%20V1.0_2026-03-22.png)
![2000 Panasonic Bottom](2000%20Panasonic/3D%20Bot_Keithley%202000%20SCAN%20Panasonic%20V1.0_2026-03-22.png)

| Property | Value |
|---|---|
| Target Instrument | Keithley 2000 |
| Relay | Panasonic TQ2-L2-5V (magnetic latching, x11) |
| Output Connector | 2x 12-position screw terminals (5.08 mm) |
| Max Voltage | 125 VDC / 125 VAC |
| Max Current | 1 A |
| Measurement | Voltage & Resistance ONLY |
| Earth Isolation | 220 V max |

Uses Panasonic magnetic latching relays - hold state without continuous coil current, reducing power and heat.

---

### 2000-SCAN Panasonic DB25

![2000 Panasonic DB25 Top](2000%20Panasonic%20DB25/3D_Keithley%202000%20SCAN%20Panasonic%20DB25%20V1.0_2026-03-22.png)
![2000 Panasonic DB25 Bottom](2000%20Panasonic%20DB25/3D%20Bot_Keithley%202000%20SCAN%20Panasonic%20DB25%20V1.0_2026-03-22.png)

| Property | Value |
|---|---|
| Target Instrument | Keithley 2000 |
| Relay | Panasonic TQ2-L2-5V (magnetic latching, x11) |
| Output Connector | DB25 female + 3-pin screw terminal (OUT B) |
| Max Voltage | 90 VDC (PCB/connector limited) |
| Measurement | Voltage & Resistance ONLY |
| Special Use | Test fixture / DUT wiring via DB25 cable |

All 10 channel H/L lines routed to a DB-25 female connector for use with custom test fixtures.

---

### 2001-SCAN Omron

![2001 Omron Top](2001%20Omron/3D_Keithley%202001%20Omron%20SCAN%20V1.0_2026-03-23.png)
![2001 Omron Bottom](2001%20Omron/3D%20bot_Keithley%202001%20Omron%20SCAN%20V1.0_2026-03-23.png)

| Property | Value |
|---|---|
| Target Instrument | Keithley 2001 |
| Relay | Omron G6SK-2-DC5V (x11) |
| Output Connector | 2x 12-position screw terminals (5.08 mm) |
| CH 1-4 Max Voltage | 110 VDC / 125 VAC rms / 1 A |
| CH 5-10 Max Current | 50 mA (solid-state input per Keithley spec) |
| Earth Isolation | 350 V peak |
| Extra Protection | GDT transient suppressors + Solid-State Relay isolation |

---

## Key Components

| Part | Description |
|---|---|
| TPIC6B595DWR (x3) | 8-bit power shift register, SOIC-20, drives relay coils directly |
| TPS3808G01DBVR | Voltage supervisor, clears relay state on power loss |
| 3511232BFRS0BNA1 | Keithley card-slot edge connector |
| TQ2-L2-5V | Panasonic 5V magnetic latching relay (Panasonic variants) |
| G6SK-2-DC5V | Omron 5V signal relay (2001 variant) |
| SMD2921-300N | GDT transient suppressor, 300V (2001 variant) |

---

## Manufacturing

Each folder contains:
- `Gerber_*.zip` - PCB fabrication files, upload directly to JLCPCB / PCBWay
- `BOM_*.xlsx` - Bill of materials with LCSC part numbers for JLCPCB SMT assembly
- `PickAndPlace_*.xlsx` - Pick and place centroid file for SMT assembly
- `SCH_*.pdf` - Schematic
- `*.epro` - EasyEDA Pro project file

**Note:** Through-hole relays and the card-edge connector (CN1) require hand soldering.

---

## Safety

> **WARNING: Omron variants — MAX 200 VDC / 125 VAC / 1 A — 350 V PEAK TO EARTH MAXIMUM**
> **Panasonic variants — MAX 125 VDC / 125 VAC / 1 A — 220 V TO EARTH MAXIMUM**
> **DB25 variant — MAX 90 VDC — VOLTAGE & RESISTANCE ONLY**
> **2001 Omron solid-state channels (CH5, CH10) — MAX 50 mA**

- Power off the Keithley completely before inserting or removing the card.
- Select the correct variant for your instrument (2000 vs. 2001).
- Voltage and resistance measurement only - not rated for current switching through the matrix.

---

## Credits

| | |
|---|---|
| Circuit & PCB Design | **Arwid Vasilev** - [github.com/arwidcool](https://github.com/arwidcool) |
| PCB Outline | **Patrick Baus** |
| EDA Tool | [EasyEDA Pro](https://pro.easyeda.com/) |

*Version 1.0 - March 2026*
