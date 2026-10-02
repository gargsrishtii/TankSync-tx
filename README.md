# TankSync TX — PCB Design

> 2-layer solar-powered transmitter board for the TankSync wireless tank monitoring system. Built around the ESP32-C3 SuperMini, RYLR998 LoRa module, and AJ-SR04M ultrasonic sensor. Designed in KiCad 10.0.

---

## Overview

TankSync TX is the transmitter-side PCB of a LoRa-based wireless tank level monitoring system. It measures tank water level using an ultrasonic sensor, monitors battery/solar power via a current sensor, and transmits data wirelessly over 868/915 MHz LoRa to the RX board.

**Key features:**
- ESP32-C3 SuperMini as the main controller (compact, Wi-Fi + BLE)
- RYLR998 LoRa UART module for long-range wireless communication
- AJ-SR04M waterproof ultrasonic sensor for tank level measurement
- CN3791 MPPT solar charging module for solar panel input
- MT3608 DC-DC boost converter for regulated power supply
- INA219 current/power sensor module for battery monitoring
- WS2812B addressable RGB LED for status indication
- AO3400/AO3401 MOSFETs for power switching
- 1N5819 Schottky diode for reverse polarity protection
- Push button (SW_PUSH_6mm) for manual control
- JST XH connectors for battery and solar panel
- GND copper pour on both layers
- 34 routed nets, 0 unconnected pads

---

## Board Specifications

| Parameter | Value |
|---|---|
| Board size | 2-layer KiCad 10.0 design |
| PCB thickness | 1.6 mm |
| Layers | 2 (F.Cu + B.Cu) |
| Design tool | KiCad 10.0 |
| Total components | 38 footprints |
| Nets | 34 |
| Unconnected pads | 0 |
| DRC violations | 7 warnings (all overridden — see notes) |
| Gerbers | Exported and verified |

---

## DRC Notes

The DRC report shows **7 warnings, 0 errors, 0 unconnected pads**. All violations are local overrides (non-critical):

| Warning | Location | Notes |
|---|---|---|
| Isolated copper fill | B.Cu GND zone | Cosmetic only — GND pour island, no electrical impact |
| Silkscreen clipped by solder mask (×5) | U2 (RYLR998) | Castellated module silkscreen overlaps pads — acceptable for this package type |
| Footprint/symbol value mismatch | C1 | C1 footprint says "C1" but symbol value is "100nF" — cosmetic, no functional impact |

**These warnings do not affect fabrication or functionality.**

---

## Schematic

> *See `/schematic/` folder for schematic export.*

Key design decisions:
- **LORA_RXD / LORA_TXD** routed from ESP32-C3 UART to RYLR998 RXD/TXD pins
- **AJ-SR04M** ultrasonic sensor connected via TRIG/ECHO GPIO lines
- **INA219** current sensor on I2C bus (SCL/SDA) for battery monitoring
- **CN3791 MPPT module** handles solar panel charging with MPPT algorithm
- **MT3608 boost converter** steps up battery voltage to regulated supply rail
- **Q4, Q5 (AO3401CI-ES)** P-channel MOSFETs for load switching
- **Q3 (AO3400)** N-channel MOSFET for signal switching
- **1N5819 Schottky diode (D1)** for reverse polarity protection on power input
- **WS2812B (D2)** status LED with 470Ω series resistor (R8)
- **100nF decoupling caps** on all module power pins

---

## Bill of Materials

| Ref | Value / Part | Package | Description |
|---|---|---|---|
| U1 | ESP32-C3 SuperMini | Module THT | Main controller — Wi-Fi + BLE |
| U2 | RYLR998 | Castellated 5-pin SMD | 868/915MHz LoRa UART module |
| U4 | CN3791 MPPT Module | Module | Solar MPPT charging controller |
| U5 | MT3608 | Module | DC-DC boost converter 2A |
| U6 | INA219 | Module | I2C current/power sensor |
| U7 | AJ-SR04M | Module | Waterproof ultrasonic distance sensor |
| D1 | 1N5819 | DO-41 THT | Schottky diode, reverse polarity protection |
| D2 | WS2812B | PLCC4 5×5mm SMD | Addressable RGB status LED |
| Q3 | AO3400 | SOT-23 SMD | N-channel MOSFET |
| Q4, Q5 | AO3401CI-ES | SOT-23 SMD | P-channel MOSFET, load switching |
| C1 | 100µF | Radial THT D6.3mm | Bulk decoupling |
| C2,C3,C4,C5,C6,C7,C9,C10 | 100nF | Disc THT | Module pin decoupling |
| C8, C11, C12 | 10µF | Radial THT D5.0mm | Power rail filtering |
| R1, R2, R7 | 100KΩ | Axial THT | Pull-up/pull-down resistors |
| R3, R4, R5, R6 | 1KΩ | Axial THT | Signal resistors |
| R8 | 470Ω | Axial THT | WS2812B data line resistor |
| R9 | 10KΩ | Axial THT | Pull-up resistor |
| R10, R11 | 4.7KΩ | Axial THT | I2C pull-up resistors (INA219 SDA/SCL) |
| J1 | Battery Connector | JST XH 2-pin | Battery input |
| J2 | Solar Connector | JST XH 2-pin | Solar panel input |
| SW1 | SW_SPST | Pin header 2-pin | Power switch |
| B1 | Push button | SW_PUSH_6mm THT | Manual control button |

---

## Repository Structure

```
TankSync-tx/
├── README.md
├── LICENSE
├── kicad/
│   ├── TankSync_TX.kicad_pro
│   ├── TankSync_TX.kicad_sch
│   ├── TankSync_TX.kicad_pcb
│   └── TankSync_TX.kicad_prl
├── gerbers/
│   └── TankSync_TX_Gerbers.zip
├── bom/
│   └── TankSync_TX_BOM.csv
└── images/
    └── (schematic, PCB layout, 3D renders)
```

---

## Gerber Files

The `/gerbers/` folder contains the complete Gerber set ready for fabrication:

| File | Layer |
|---|---|
| TankSync_TX-F_Cu.gbr | Front copper |
| TankSync_TX-B_Cu.gbr | Back copper |
| TankSync_TX-F_Silkscreen.gbr | Front silkscreen |
| TankSync_TX-B_Silkscreen.gbr | Back silkscreen |
| TankSync_TX-F_Mask.gbr | Front solder mask |
| TankSync_TX-B_Mask.gbr | Back solder mask |
| TankSync_TX-F_Paste.gbr | Front solder paste |
| TankSync_TX-B_Paste.gbr | Back solder paste |
| TankSync_TX-Edge_Cuts.gbr | Board outline |
| TankSync_TX-PTH.drl | Plated through-hole drill file |
| TankSync_TX-NPTH.drl | Non-plated through-hole drill file |
| TankSync_TX-job.gbrjob | Gerber job file |

Recommended fab services: JLCPCB, PCBWay, OSH Park.

---

## Custom Library Dependency

This design uses a custom KiCad component:

- `MyLibrary:RYLR998_Castellated_5P` — REYAX RYLR998 LoRa module

**To open this project in KiCad**, add the custom library first:
👉 [gargsrishtii/kicad-parts-library](https://github.com/gargsrishtii/kicad-parts-library)

---

## Related Repository

- **TankSync RX PCB** — [gargsrishtii/TankSync-rx](https://github.com/gargsrishtii/TankSync-rx)

---

## License

This hardware design is released under **CERN-OHL-P v2** (Permissive Open Hardware License).
Free to use, modify, and manufacture — no share-alike requirement.
See [LICENSE](LICENSE) for full terms.

---

*Part of the TankSync wireless tank monitoring system.*
*Custom KiCad library: [gargsrishtii/kicad-parts-library](https://github.com/gargsrishtii/kicad-parts-library)*
