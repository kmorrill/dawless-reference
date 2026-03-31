# Teenage Engineering OP-XY

## Overview
- **Type**: Portable synthesizer / sequencer / sampler
- **Voices**: 24 polyphonic
- **Tracks**: 16 (8 instrument + 8 auxiliary)
- **Sequencer**: 64 steps per pattern, parameter locks
- **Engines**: 8 synth engines (cluster, digital, DNA, drwave, FM, noise, string, VA)
- **Scenes**: 99 total

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | MIDI device mode, audio interface, file transfer |
| Built-in speaker | Out | Mono |
| 3.5mm headphone | Out | Stereo |
| Line in | In | 3.5mm stereo |

## MIDI Implementation

### Channels
- **Tracks 1-8**: Instrument tracks (channels 1-8)
- **Tracks 9-16**: Auxiliary tracks (channels 9-16)
- USB MIDI only (no DIN MIDI)

### Key CCs
| CC | Function | Range |
|----|----------|-------|
| 7 | Volume | 0-127 |
| 10 | Pan | 0-127 (64=center) |
| 20 | Attack | 0-127 |
| 21 | Decay | 0-127 |
| 22 | Sustain | 0-127 |
| 23 | Release | 0-127 |
| 32 | Filter Cutoff | 0-127 |
| 33 | Filter Resonance | 0-127 |
| 38 | FX I Send (Delay) | 0-127 |
| 39 | FX II Send (Reverb) | 0-127 |
| 80-83 | Synth Params P1-P4 | 0-127 |
| 106 | Remote Key Press | Special |
| 107 | Remote Key Release | Special |

Full CC map: [midi-cc-map.md](midi-cc-map.md)
Remote keys: [cc106-remote-keys.md](cc106-remote-keys.md)

### Program Change
- Supports standard MIDI Program Change (0xC0)
- Bank MSB (CC 0) and Bank LSB (CC 32) can be sent before Program Change
- Range: PC 0-127

### Project Selection (CC 86)
- **CC 86** selects projects by index
- Projects must be named with a 3-digit prefix (e.g., `000MyProject.xy`, `005MyProject.xy`)
- CC 86 value 0-127 maps to the prefix number
- Requires external USB MIDI adapter on AUX Type A port (USB-C is power/MTP only)

### Drum Kit
MIDI notes 53-76 (24 slots). See [drum-note-map.md](drum-note-map.md).

## Sync
- MIDI clock send/receive at **24 PPQN** (standard MIDI clock)
- Transport: Start (0xFA), Stop (0xFC), Clock (0xF8)
- Optional Song Position Pointer (0xF2) support
- USB MIDI only (no analog clock)

## Engines
8 synth engines, each with P1-P4 parameter meanings. See [synth-engines.md](synth-engines.md).

## File Formats
- **`.xy`** — Binary project file ([xy-format-readme.md](formats/xy-format-readme.md))
- **`.opxyloop`** — JSON loop interchange format ([opxyloop-format-spec.md](formats/opxyloop-format-spec.md))
- **`.preset`** — Folder with patch.json + samples ([preset-format.md](formats/preset-format.md))

## Non-Automatable Controls
These **cannot** be controlled via MIDI CC — require physical interaction:
- Engine/preset selection
- Sample recording/assignment
- Play mode (poly/mono/legato)
- Portamento time
- Pitch bend range
- Filter type selection
- LFO type and routing
- Velocity response (system settings)

Only **Params 1-4** (CC 80-83) are automatable per-engine synth parameters.

Full details: [non-automatable-controls.md](non-automatable-controls.md)

## Limitations
See [device-limits.md](device-limits.md).

## Physical Controls
See [hardware-control-map.md](hardware-control-map.md) and [manual-notes.md](manual-notes.md).

## 3rd-Party Repos & Libraries

| Repo | Language | What it does |
|------|----------|-------------|
| [kmorrill/TonicMIDI](https://github.com/kmorrill/TonicMIDI) | TypeScript | Browser-based sequencer/controller for OP-XY (Web MIDI, transport management, pattern playback) |
| [kmorrill/op-xy-vibing](https://github.com/kmorrill/op-xy-vibing) | Python | Real-time playback engine with conductor, MIDI clock, Program Change support |
| [kmorrill/xy-format](https://github.com/kmorrill/xy-format) | Python | .xy binary project file reverse engineering, CC 86 project selection tools |
| [kmorrill/xy-remix](https://github.com/kmorrill/xy-remix) | Python | Preset tools (drum kits, multisamples, stems) |
