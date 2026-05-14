# DVSI Serial Packet Protocol Reference

The AMBE-3000R communicates via a simple 3-byte framed packet protocol over
UART at 460800 baud, 8N1.

## Packet Structure

```
Byte 0: Field ID   (0x61 = start-of-packet marker)
Byte 1: Packet type
Byte 2: Payload length (N bytes follow)
Bytes 3..3+N-1: Payload
```

## Key Packet Types (Receive direction, host → chip)

| Type | Name | Description |
|------|------|-------------|
| 0x00 | PKT_RESET | Reset codec; chip responds with version string |
| 0x01 | PKT_CHANNEL | Set vocoder channel (AMBE+2, AMBE+2 Pro, IMBE, etc.) |
| 0x02 | PKT_SPEECHD | Send PCM audio samples for encoding |
| 0x13 | PKT_AMBED | Send AMBE frame bytes for decoding |
| 0x40 | PKT_PRODID | Request product ID / version string |

## Key Packet Types (Transmit direction, chip → host)

| Type | Name | Description |
|------|------|-------------|
| 0x02 | PKT_SPEECHD | Decoded PCM samples |
| 0x13 | PKT_AMBED | Encoded AMBE frame bytes |
| 0x00 | PKT_READY | Response to PKT_RESET; payload = version string |

## PCM Format

- Sample rate: 8000 Hz
- Bit depth: 16-bit signed PCM
- Frame size: 160 samples (20 ms) per packet

## AMBE+2 Frame Size

- 72 bits (9 bytes) per 20 ms frame

## Vocoder Channel IDs

| ID | Vocoder | Used in |
|----|---------|---------|
| 0x00 | AMBE+2 | DMR, YSF (default rate) |
| 0x01 | AMBE+2 Pro | YSF (high quality) |
| 0x04 | IMBE | P25 Phase 1, D-Star |

## References

- DVSI AMBE-3000R product datasheet (obtain from DVSI under NDA or with chip
  purchase)
- [dv3000d](https://github.com/juribeparada/MMDVM_HS) — open source daemon that
  speaks this protocol
- [BlueDV](https://www.pa7lim.nl/bluedv-windows/) — reference consumer
  implementation
