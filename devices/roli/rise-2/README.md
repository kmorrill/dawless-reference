# ROLI Seaboard Rise 2

## Overview
- **Type**: MPE controller with continuous touch surface
- **Keys**: 49 keywaves (silicone, not traditional keys)
- **Expression**: 5 dimensions of touch per note
- **Internal sound**: None — controller only

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | Out | MIDI (MPE) |
| Bluetooth LE | Out | MIDI (MPE), wireless |

No DIN MIDI. No audio output. No CV.

## 5 Dimensions of Touch (5D)
| Dimension | MIDI Message | Description |
|-----------|-------------|-------------|
| Strike | Note-on velocity | How hard you hit the keywave |
| Press | Channel aftertouch | Continuous downward pressure |
| Glide | Pitch bend (per-channel) | Left/right slide for pitch bending |
| Slide | CC 74 | Forward/backward on keywave (timbre) |
| Lift | Note-off velocity | How quickly you release |

## MPE Implementation
- Lower Zone: Channel 1 = manager, Channels 2-16 = member channels
- Default pitch bend range: 48 semitones
- Configurable via ROLI Dashboard software
- Sensitivity curves adjustable per dimension

## Configuration (ROLI Dashboard)
- Per-dimension sensitivity curves
- MPE zone setup
- Pitch bend range
- CC assignments
- Firmware updates
- Works over USB or Bluetooth

## Limitations
- No standalone sound generation
- Bluetooth adds slight latency vs USB
- Silicone surface requires different playing technique than traditional keys
- Limited to MPE-compatible destinations for full expression
