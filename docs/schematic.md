# openRigDVStick — Functional Block Description

```
USB-C (J1)
  │
  ├─ VBUS (5 V) ──► LDO 3.3 V (U3) ──► 3.3 V rail
  │                                         │
  │                               ┌─────────┴────────┐
  │                               │                  │
  │                            CP2102N (U2)    AMBE-3000R (U1)
  │                               │
  ├─ D+ / D− ──► ESD (D1) ──────►│ USB FS
                                  │
                                  │ UART TX/RX (460800 baud)
                                  └──────────────────► AMBE-3000R (U1)
                                                            │
                                                        UART RX/TX
```

## Signal Connections

### USB-C (J1) → CP2102N (U2)

| J1 pin | U2 pin | Signal |
|--------|--------|--------|
| D+ | D+ | USB data (via ESD clamp) |
| D− | D− | USB data (via ESD clamp) |
| VBUS | VDD | 5 V supply (to LDO input) |
| GND | GND | |
| CC1, CC2 | — | 5.1 kΩ pull-down to GND each |

### CP2102N (U2) → AMBE-3000R (U1)

| U2 pin | U1 pin | Signal |
|--------|--------|--------|
| TXD | RXD | UART TX → AMBE RX |
| RXD | TXD | UART RX ← AMBE TX |
| GND | GND | |

CP2102N baud rate configured to **460800** by host driver at open time.
AMBE-3000R default boot baud is 460800.

### Status LED

CP2102N `SUSPEND\` or `GPIO.0` → R3 (33 Ω) → LED1 → GND.
LED lights when USB is active / not suspended.
