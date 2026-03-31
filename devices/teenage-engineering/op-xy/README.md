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

### Drum Kit
MIDI notes 53-76 (24 slots). See [drum-note-map.md](drum-note-map.md).

## Engines
8 synth engines, each with P1-P4 parameter meanings. See [synth-engines.md](synth-engines.md).

## File Formats
- **`.xy`** — Binary project file ([xy-format-readme.md](formats/xy-format-readme.md))
- **`.opxyloop`** — JSON loop interchange format ([opxyloop-format-spec.md](formats/opxyloop-format-spec.md))
- **`.preset`** — Folder with patch.json + samples ([preset-format.md](formats/preset-format.md))

## Limitations
See [device-limits.md](device-limits.md) and [non-automatable-controls.md](non-automatable-controls.md).

## Physical Controls
See [hardware-control-map.md](hardware-control-map.md) and [manual-notes.md](manual-notes.md).
