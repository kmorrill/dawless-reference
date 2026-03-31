# ROLI Blocks

## Overview
- **Type**: Modular MPE controller system
- **Modules**: Lightpad Block (touch surface), Seaboard Block (keywaves), Loop Block (controls), Live Block (controls)
- **Expression**: 5 dimensions of touch (same as Rise 2)
- **Magnets**: Modules snap together magnetically, auto-configure

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | Out | MIDI (MPE), charging |
| Bluetooth LE | Out | MIDI (MPE), wireless |
| DNA Connectors | Inter-module | Magnetic, data + power between blocks |

## Lightpad Block
- 15x15 LED grid with continuous pressure-sensitive touch surface
- Not a button matrix — continuous XY + pressure sensing
- RGB LEDs for visual feedback
- Can be programmed as drum pads, XY controller, fader bank, etc.

## Seaboard Block
- 24 keywaves (2 octaves)
- Same 5D touch as Rise 2 but smaller form factor
- MPE output

## 5 Dimensions of Touch
| Dimension | MIDI Message | Description |
|-----------|-------------|-------------|
| Strike | Note-on velocity | Impact force |
| Press | Channel aftertouch | Sustained pressure |
| Glide | Pitch bend | Left/right movement |
| Slide | CC 74 | Forward/backward movement |
| Lift | Note-off velocity | Release speed |

## Configuration (ROLI Dashboard / BLOCKS app)
- Per-dimension sensitivity
- Surface layout (drum pads, keys, faders, etc.)
- MPE settings
- LED colors and feedback modes
- Firmware updates

## Limitations
- Battery life varies (Lightpad ~4hrs, Seaboard ~4hrs)
- Bluetooth LE adds slight latency
- ROLI software required for configuration
- Limited community support since ROLI's restructuring (now Luminary)
