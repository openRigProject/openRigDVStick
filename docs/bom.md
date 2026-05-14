# openRigDVStick Bill of Materials

> Annotated BOM. Machine-readable CSV will be generated from KiCad once the
> schematic is complete.

| Ref | Part | Value / PN | Package | Notes |
|-----|------|-----------|---------|-------|
| U1 | AMBE-3000R | AMBE-3000R | 48-LQFP | DVSI licensed AMBE+2/AMBE+2Pro/IMBE codec; UART 460800 baud; 3.3 V |
| U2 | CP2102N-A02-GQFN24 | CP2102N | QFN-24 | USB 2.0 FS CDC bridge; no driver needed on Linux/macOS; VBUS detect; internal oscillator |
| U3 | MIC5219-3.3YM5 | 3.3 V LDO | SOT-23-5 | 5 V VBUS in → 3.3 V out, 500 mA; powers U1 + U2 |
| J1 | USB-C receptacle | USB4085-GF-A | USB-C 2.0 mid-mount | Bus power + data; CC resistors for USB 2.0 power negotiation |
| D1 | ESD protection | USBLC6-2SC6 | SOT-23-6 | USB D+/D− ESD suppressor |
| LED1 | Status LED | Green, 0402 | 0402 | Activity; driven from CP2102N GPIO |
| R1, R2 | CC pull-down | 5.1 kΩ, 0402 | 0402 | USB-C CC1/CC2 for 5 V/500 mA UFP |
| C1, C2 | Bulk cap | 10 µF, 0402 | 0402 | VBUS and 3.3 V rail |
| C3–C8 | Decoupling | 100 nF, 0402 | 0402 | Per-IC supply decoupling |
| R3 | LED current limit | 33 Ω, 0402 | 0402 | ~10 mA at 3.3 V |

## Notes

- **CP2102N vs FT232RL:** CP2102N chosen for lower cost, smaller QFN package,
  built-in oscillator, and driver-free operation on modern OS. FT232RL is a
  drop-in alternative if preferred.
- **AMBE-3000R sourcing:** Available from DVSI directly or via authorised
  distributors. Confirm availability before committing PCB layout.
- **PCB size target:** ~18 × 55 mm (USB stick form factor), 2-layer FR4.
