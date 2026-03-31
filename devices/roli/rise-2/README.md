# ROLI Seaboard Rise 2

## Overview
- **Type**: MPE controller with continuous touch surface
- **Keys**: 49 keywaves (silicone, not traditional keys)
- **Expression**: 5 dimensions of touch per note
- **Internal sound**: None — controller only

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | Out | MIDI (MPE), charging |
| Bluetooth 4.0 LE | Out | MIDI (MPE), wireless |
| 3.5mm TRS | Out | MIDI out |
| 6.35mm TRS | In | Footpedal input |

No DIN MIDI. No audio output. No CV. 8-hour rechargeable battery (3-hour charge). Weight: 5.5 kg.

## 5 Dimensions of Touch (5D)
| Dimension | MIDI Message | Description |
|-----------|-------------|-------------|
| Strike | Note-on velocity | How hard you hit the keywave |
| Press | Channel aftertouch | Continuous downward pressure |
| Glide | Pitch bend (per-channel) | Left/right slide for pitch bending |
| Slide | CC 74 | Forward/backward on keywave (timbre) |
| Lift | Note-off velocity | How quickly you release |

## MPE Implementation
- Lower Zone: Channel 1 = manager, Channels 2-16 = member channels (up to 15 per-note channels)
- Default pitch bend range: ±48 semitones per-note, ±2 semitones on master channel
- Three modes: MPE, Multi-Channel, Single-Channel (for non-MPE instruments)
- Configurable via ROLI Dashboard software
- Sensitivity curves adjustable per dimension (5 response curves each)
- Three physical Touch Faders on surface adjust Glide, Slide, and Press sensitivity in real time

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
