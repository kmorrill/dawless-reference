# Arturia MiniFreak

## Overview
- **Type**: Hybrid polyphonic synthesizer
- **Voices**: 6
- **Oscillators**: 2 digital oscillators (22 types) + analog filter
- **Keyboard**: 37 keys, velocity + aftertouch
- **Sequencer**: 4 tracks, 64 steps
- **Arpeggiator**: Built-in with multiple modes
- **Effects**: 3 FX slots (chorus, flanger, phaser, delay, reverb, etc.)

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI + audio interface |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| Audio Out | Out | 2x 1/4" (L/R) |
| Headphone | Out | 1/4" stereo |
| Clock In | In | 3.5mm |
| Clock Out | Out | 3.5mm |
| Expression | In | 1/4" TRS |

## MIDI Implementation

### Channel
- Default: Channel 1 (configurable 1-16)
- MPE: Not supported

### Key CCs
| CC | Parameter |
|----|-----------|
| 1 | Mod wheel |
| 2 | Osc 1 Type |
| 3 | Osc 1 Shape |
| 4 | Osc 1 Wave |
| 9 | Osc 2 Type |
| 10 | Osc 2 Shape |
| 11 | Osc 2 Wave |
| 23 | Filter Cutoff |
| 24 | Filter Resonance |
| 83 | FX 1 Amount |
| 85 | FX 2 Amount |
| 87 | FX 3 Amount |

Full CC map available via Arturia MIDI Control Center.

### Program Change
- 512 preset slots (256 factory + 256 user)
- Bank select (CC 0) + Program Change

### Sync
- MIDI clock send/receive
- Analog clock in/out (3.5mm, configurable ppqn)

## Software
- **Arturia MIDI Control Center** — patch management, MIDI mapping, firmware updates
- **MiniFreak V** — software version of the synth (included), also serves as patch editor

## Limitations
- No MPE support
- USB audio is stereo only (not multi-channel)
- Sequencer limited to 64 steps per track
- Expression pedal input is single (no second pedal input)
