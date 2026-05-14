# openRigDVStick

Open source USB dongle providing a licensed DVSI AMBE-3000R hardware codec for
DMR and YSF voice encode/decode. A fully open alternative to the ThumbDV / DVStick30.

Plug into any USB port — the host sees a standard CDC serial device. Compatible
with openRigOS, openRigConsole, and any software that speaks the DVSI serial
packet protocol.

## Features

- **AMBE-3000R** — licensed DVSI hardware codec; supports AMBE+2, AMBE+2 Pro,
  and IMBE; covers both DMR and YSF (D-Star, P25 Phase 1 via IMBE)
- **USB-C connector** — bus-powered from USB 5 V; no external supply needed
- **CP2102N USB bridge** — presents as `/dev/ttyUSBx` (Linux) or `COMx`
  (Windows); no custom driver needed on Linux/macOS
- **Status LED** — activity indicator on USB data lines
- **Compact dongle form factor** — exposed USB-C plug, fits a standard USB-A
  adapter if needed

## Repository Layout

```
openRigDVStick/
  kicad/          KiCad schematic and PCB layout
  fabrication/    Gerbers, BOM, CPL (for JLCPCB / PCBWay)
  docs/
    schematic.md  Functional block description
    bom.md        Annotated bill of materials
    assembly.md   Assembly and bring-up guide
    protocol.md   DVSI serial packet protocol reference
```

## Status

> **Design phase** — schematic in progress.

## Hardware Overview

| Section | Key ICs | Notes |
|---------|---------|-------|
| USB bridge | CP2102N-A02-GQFN24 | USB 2.0 Full Speed CDC; 460800 baud to AMBE |
| AMBE codec | AMBE-3000R (48-LQFP) | UART at 460800 baud; 3.3 V |
| Power | MIC5219-3.3YM5 LDO | 5 V USB VBUS → 3.3 V, 500 mA |

## Compatibility

Works anywhere the ThumbDV / DVStick30 works:

- **openRigOS** — auto-detected via udev; used by `openrig-ambe` decoder service
- **openRigConsole** — audio encode/decode via `AudioService` ConnectRPC API
- **BlueDV, MMDVM\_Bridge, DV3000** — standard DVSI serial protocol

## Licensing Note

The AMBE-3000R is a commercial component sold by DVSI. Purchasing the chip
constitutes the required hardware license for AMBE+2 encoding/decoding.

## License

Hardware design files: [CERN Open Hardware Licence v2 – Permissive](https://ohwr.org/cern_ohl_p_v2.txt)

Documentation: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
