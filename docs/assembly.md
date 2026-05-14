# openRigDVStick Assembly and Bring-Up Guide

> This guide will be expanded once the PCB design is complete.

## PCB Specifications

- Size: ~18 × 55 mm (target)
- Layers: 2 (FR4, 1.6 mm)
- Surface finish: ENIG (recommended for fine-pitch QFN/LQFP)
- Min trace/space: 0.1 / 0.1 mm

## Assembly Notes

- **U1 (AMBE-3000R, 48-LQFP):** 0.5 mm pitch — reflow soldering recommended.
  Apply solder paste with stencil; inspect with 10× loupe or microscope.
- **U2 (CP2102N, QFN-24):** Exposed thermal pad must be soldered to ground
  plane; use stencil + reflow.
- **J1 (USB-C):** Mid-mount receptacle — verify courtyard clearance during
  layout.

## Bring-Up Checklist

- [ ] Visual inspection — no solder bridges on U1/U2
- [ ] 3.3 V rail voltage check before first USB connection
- [ ] Plug into USB port — OS enumerates as `Silicon Labs CP210x USB to UART Bridge`
- [ ] Linux: `/dev/ttyUSB0` appears; macOS: `/dev/tty.usbserial-*`
- [ ] Send `PKT_RESET` (0x61, 0x00, 0x00) at 460800 baud — expect version
  response from AMBE-3000R (product string contains "AMBE3000")
- [ ] Test encode/decode round-trip with `dv3000d` or openRigOS `openrig-ambe`

## udev Rule (Linux / openRigOS)

openRigOS ships a udev rule that detects the CP2102N VID/PID and creates a
stable symlink:

```
SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", \
  SYMLINK+="ttyDVStick", TAG+="openrig_dvstick"
```

This triggers the `openrig-ambe` service to start automatically when the dongle
is plugged in.
