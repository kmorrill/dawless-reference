# Roland AIRA Compact J-6

## Overview
- **Type**: Chord synthesizer with phrase sequencer
- **Sound engine**: Juno-style synth with 64 sound presets
- **Chords**: Built-in chord sets (registered chord progressions mapped to keyboard buttons)
- **Phrase function**: Auto-generates phrases/arpeggios based on input chords, with selectable style and variation
- **Sequencer**: 8-step chord sequencer, 64 patterns (8 banks x 8)
- **Controls**: Filter (cutoff + resonance), Envelope (attack + release), Delay, Reverb, Style, Variation, Hold
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
- **Synth channel**: Default 1, configurable 1-16
- **Program Change channel**: Default 16, configurable 1-16

### Key CCs
| CC | Parameter | Notes |
|----|-----------|-------|
| 64 | Hold (Sustain) | Only CC recognized by the J-6 |

### Other Messages
| Message | TX | RX | Notes |
|---------|----|----|-------|
| Note On/Off | Yes | Yes | Range 0-127, velocity supported |
| Pitch Bend | No | No | |
| Aftertouch | No | No | |
| Program Change | Yes | Yes | Range 0-63 (64 patterns); gated by txPc/rxPc settings |
| SysEx | No | No | |

### Pattern Change Timing
- **P.chg**: Controls when pattern changes take effect (end of step vs. after N beats)

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
- **Continue**: Not supported
- **MIDI Thru**: Configurable
- **Sync clocks per beat**: 1, 2, 3, 4, 6, 8, 12, 24

## Limitations
- Only one CC recognized (CC 64 Hold) -- no remote control of filter, envelope, delay, reverb, or phrase parameters via MIDI
- No pitch bend
- No aftertouch
- No SysEx
- No Song Position Pointer / Song Select
- No Continue message support

## Detailed Docs
- [MIDI Implementation (external control reference)](midi-implementation.md)
- [Owner's Manual](pdfs/J-6-manual.md)
