# openRigDVStick — Net Connection Reference

Complete net list for schematic entry. All net names are canonical — use these
exactly in KiCad to ensure ERC passes cleanly.

## Power Nets

| Net | Source | Consumers |
|-----|--------|-----------|
| `VBUS` | J1 pin A4/B9 (USB-C VBUS) | U3 input |
| `+3V3` | U3 output | U1 (AMBE), U2 (CP2102N), pull-ups |
| `GND` | J1 pin A1/B1/A12/B12 | All ICs, decoupling caps |

## USB Differential Pair

| Net | J1 pin | U2 pin | D1 pin |
|-----|--------|--------|--------|
| `USB_D+` | A6 / B6 | D+ | A (via 22 Ω series R) |
| `USB_D-` | A7 / B7 | D- | K (via 22 Ω series R) |

## UART Bridge → AMBE Codec

| Net | U2 (CP2102N) pin | U1 (AMBE-3000R) pin | Notes |
|-----|-----------------|---------------------|-------|
| `UART_TX` | TXD | RXD | CP210x → AMBE |
| `UART_RX` | RXD | TXD | AMBE → CP210x |
| `UART_RTS` | RTS | CTS | Optional flow control |
| `UART_CTS` | CTS | RTS | Optional flow control |

Baud rate: **460800**, 8N1. Configured by host driver at port open.

## AMBE-3000R Power and Configuration Pins

| Net | U1 pin(s) | Notes |
|-----|-----------|-------|
| `+3V3` | VDD (multiple) | Decouple each VDD pin with 100 nF to GND |
| `GND` | GND (multiple) | Pour under device |
| `RESET_N` | RESET | 10 kΩ pull-up to +3V3; optional reset button to GND |
| `BOOT_SEL` | BOOT | Pull to GND for normal UART boot mode |

Per AMBE-3000R datasheet: place 100 nF + 10 µF bulk cap on each VDD cluster.

## CP2102N Power and Configuration

| Net | U2 pin | Notes |
|-----|--------|-------|
| `VBUS` | VDD (power input) | Feeds LDO and CP2102N VDD directly (CP210x is 3.3 V tolerant on I/O but needs 3.3 V supply) |
| `+3V3` | VDD | Supply from U3 |
| `GND` | GND | |
| `+3V3` | REGIN | Tie to +3V3 if using internal 3.3 V regulator bypass |
| `USB_D+` | D+ | |
| `USB_D-` | D- | |
| `LED_ACT` | GPIO.0 | → R_LED (33 Ω) → LED1 → GND |

CP2102N VID/PID: `10C4:EA60` (Silicon Labs default). Program serial number
string via Simplicity Studio to distinguish from other CP210x dongles.

## USB-C Connector (J1)

| J1 pin | Net | Notes |
|--------|-----|-------|
| A4, B9 | `VBUS` | 5 V bus power |
| A1, B1, A12, B12 | `GND` | |
| A6 | `USB_D+` | |
| B6 | `USB_D+` | Tied together (USB 2.0) |
| A7 | `USB_D-` | |
| B7 | `USB_D-` | Tied together (USB 2.0) |
| A5, B5 | `CC1`, `CC2` | 5.1 kΩ pull-down each to GND (UFP, 5 V/500 mA) |
| A8, B8 | `SBU1`, `SBU2` | Leave unconnected (no alt mode) |

## ESD Protection (D1 — USBLC6-2SC6)

| D1 pin | Net |
|--------|-----|
| 1 (I/O1) | `USB_D-` |
| 2 (GND) | `GND` |
| 3 (I/O2) | `USB_D+` |
| 4 (I/O3) | `USB_D+` |
| 5 (VCC) | `VBUS` |
| 6 (I/O4) | `USB_D-` |

## LDO Regulator (U3 — MIC5219-3.3YM5, SOT-23-5)

| U3 pin | Net |
|--------|-----|
| 1 (IN) | `VBUS` |
| 2 (GND) | `GND` |
| 3 (EN) | `VBUS` (always on) |
| 4 (BP) | 100 nF to GND |
| 5 (OUT) | `+3V3` |

Output: 10 µF + 100 nF to GND.

## Decoupling Summary

| Location | Value | Net |
|----------|-------|-----|
| U1 each VDD | 100 nF | `+3V3` to `GND` |
| U1 bulk | 10 µF × 2 | `+3V3` to `GND` |
| U2 VDD | 100 nF + 10 µF | `+3V3` to `GND` |
| U3 output | 100 nF + 10 µF | `+3V3` to `GND` |
| VBUS (J1 entry) | 10 µF | `VBUS` to `GND` |

## Form Factor Note

The AMBE-3000R is a **128-pin LQFP, 14×14mm, 0.4mm pitch**. A traditional
USB-A stick form factor (~18 mm wide) is too narrow. Target PCB size:
**30 × 60 mm**, 2-layer FR4, with a short USB-C cable or right-angle USB-C
connector at one end.
