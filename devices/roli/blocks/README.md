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

## MPE Implementation
- Channel 1 = global/master channel (preset changes, pedal data)
- Channels 2-16 = per-note member channels (up to 15 simultaneous expressive notes)
- Default pitch bend: ±48 semitones per-note, ±2 semitones on master channel
- Three modes: MPE, Multi-Channel, Single-Channel (for non-MPE instruments)

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

## Physical Controls per Module

| Module | Controls |
|--------|----------|
| **Lightpad Block** | 15x15 LED grid + continuous XYZ multi-touch + 1 mode button |
| **Seaboard Block** | 24 keywaves with 5DOF + LED strip |
| **Loop Block** | 10 buttons: mode, volume, click, snap, back, play/pause, record, learn, down, up |
| **Live Block** | 10 buttons: mode, volume, scale, chord, arp, sustain, octave, love, down, up |
| **Touch Block** | 10 sensitivity buttons |
| **Developer Block** | 8 generic buttons + down + up |

## SysEx / USB Protocol

BLOCKS devices use a **proprietary packed-bit protocol tunneled inside SysEx** (manufacturer ID `0x00 0x21 0x10`). This is NOT standard SysEx — it's a custom binary format carrying:
- Topology messages (which blocks are connected, orientation)
- Touch events (12-bit X/Y, 8-bit Z)
- Button events
- Config changes (128 configurable items)
- Firmware updates
- Littlefoot program messaging

### Littlefoot Scripting
A C-like language that runs **on the hardware** with callbacks for touch, buttons, MIDI, and a full drawing API for the 15x15 LED grid. Factory scripts include: RISE Controller, Drum Block, Fader Block, Mixer Block, Ableton Live Control.

## Configuration (ROLI Dashboard / BLOCKS app / Luminary)
- Per-dimension sensitivity
- Surface layout (drum pads, keys, faders, etc.)
- MPE settings
- LED colors and feedback modes
- Littlefoot script upload
- 128 config items (MIDI, touch sensitivity, scale/chord/arp, graphics)
- Firmware updates

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [WeAreROLI/BLOCKS-SDK](https://github.com/WeAreROLI/BLOCKS-SDK) | Official C++17 SDK (ISC license). Core classes: Block, TouchSurface, LEDGrid, LEDRow, ControlButton, TopologySource |
| [microtonal-seaboard](https://github.com/search?q=microtonal-seaboard) | Split-key microtonal mappings using Slide dimension |
| [agraef/myblocks](https://github.com/agraef/myblocks) | C + Lua interface + Pure Data external for BLOCKS |
| [Lightpad-Block-Sandbox](https://github.com/search?q=Lightpad-Block-Sandbox) | Littlefoot script collection and testing environment |

## Limitations
- Battery life varies (Lightpad ~4hrs, Seaboard ~4hrs)
- Bluetooth LE adds slight latency
- ROLI software required for configuration
- Limited community support since ROLI's restructuring (now Luminary)
- Proprietary SysEx protocol — not human-readable or documented beyond the SDK
