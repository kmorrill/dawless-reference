# Roland AIRA Compact T-8

## Overview
- **Type**: Drum machine and bass synthesizer
- **Rhythm section**: 6 instruments -- Bass Drum, Snare Drum, Hand Clap, Tom, Closed Hi-hat, Open Hi-hat
- **Bass section**: Monophonic bass synth with Cutoff, Resonance, Envelope Mod, Decay, Pitch controls
- **Effects**: Delay, Reverb, Accent (per-step for rhythm and bass)
- **Sequencer**: Step sequencer with 64 patterns (16 steps per pattern, expandable), accent and slide per step
- **Firmware**: v1.02

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | Class-compliant USB MIDI + USB audio |
| 3.5mm TRS MIDI IN | In | TRS-to-TRS or TRS-to-DIN cables |
| 3.5mm TRS MIDI OUT | Out | TRS-to-TRS or TRS-to-DIN cables |
| SYNC IN | In | Mono 3.5mm, overrides MIDI clock setting |
| SYNC OUT | Out | Mono 3.5mm |
| MIX IN | In | Stereo 3.5mm audio |
| MIX OUT | Out | Stereo 3.5mm audio + headphones |

## MIDI Implementation

### Channels
- **Bass channel**: Default 2, configurable 1-16 or OFF
- **Rhythm channel**: Default 10, configurable 1-16 or OFF
- **Program Change channel**: Default 16, configurable 1-16

### Rhythm Note Map (RX)
| Instrument | RX Note Numbers |
|------------|----------------|
| Bass Drum | 35, 36 |
| Snare Drum | 38, 40 |
| Hand Clap | 48, 50 |
| Tom | 45, 47 |
| Closed Hi-hat | 42, 44 |
| Open Hi-hat | 46 |

### Rhythm Note Map (TX)
| Instrument | TX Note Number |
|------------|---------------|
| Bass Drum | 36 |
| Snare Drum | 38 |
| Hand Clap | 50 |
| Tom | 47 |
| Closed Hi-hat | 42 |
| Open Hi-hat | 46 |

### Bass Notes
- **RX note range**: 12-96 (C0 through C7)
- **Velocity**: Supported

### Other Messages
| Message | TX | RX | Notes |
|---------|----|----|-------|
| Note On/Off | Yes | Yes | Velocity supported |
| Control Change | No | No | No CC control of any parameters |
| Pitch Bend | No | No | |
| Aftertouch | No | No | |
| Program Change | Yes | Yes | Range 0-63; gated by tXPc/rXPc settings |
| SysEx | No | No | |

## Sync
| Mode | Description |
|------|-------------|
| AUTO | Accept incoming clocks from any source |
| Int | Internal clock only |
| MIDI | Sync to MIDI IN only |
| USB | Sync to USB MIDI only |

- **Clock TX/RX**: Yes
- **Start TX/RX**: Yes
- **Stop TX/RX**: Yes
- **Continue**: RX only (not transmitted)
- **MIDI Thru**: Configurable (default On)
- **Sync clocks per beat**: 1, 2, 3, 4, 6, 8, 12, 24

## Limitations
- No CC control of any parameters (tune, decay, level, cutoff, resonance, etc. are not MIDI-controllable)
- No pitch bend
- No aftertouch
- No SysEx
- No Song Position Pointer / Song Select

## Detailed Docs
- [MIDI Implementation (external control reference)](midi-implementation.md)
- [Owner's Manual](pdfs/T-8-manual.md)
