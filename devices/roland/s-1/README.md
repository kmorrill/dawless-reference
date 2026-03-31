# Roland AIRA Compact S-1

## Overview
- **Type**: Monophonic/polyphonic synthesizer with step sequencer
- **Polyphony**: Mono, Unison, Poly (up to 4 voices), Chord mode
- **Oscillator**: Square + Sawtooth + Sub oscillator + Noise, with OSC Draw and OSC Chop waveform shaping
- **Filter**: Low-pass with resonance, LFO mod, envelope mod, keyboard follow
- **Envelope**: ADSR with gate/envelope amp modes and multiple trigger modes
- **LFO**: 6 waveforms (saw, inverted saw, triangle, square, random, noise), syncable to tempo
- **Effects**: Delay, Reverb, Chorus (4 types)
- **Sequencer**: Step sequencer with motion recording (up to 8 parameter motions per pattern), 64 patterns (8 banks x 8)
- **Special features**: D-MOTION (tilt control), arpeggiator, step loop, key transpose, riser, master probability
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
- **Synth channel**: Default 3, configurable 1-16
- **Program Change channel**: Default 16, configurable 1-16

### Key CCs
| CC | Parameter | Notes |
|----|-----------|-------|
| 1 | Modulation Wheel | LFO sine modulation |
| 3 | LFO Rate | |
| 5 | Portamento Time | |
| 10 | Pan | |
| 11 | Expression | |
| 12 | LFO Waveform | 0-5: Saw/InvSaw/Tri/Sqr/Rnd/Noise |
| 13 | OSC LFO (pitch mod) | |
| 14 | OSC Range (octave) | |
| 15 | Pulse Width | |
| 16 | PWM Source | 0=Env, 1=Manual, 2=LFO |
| 17 | LFO Modulation Depth | |
| 18 | OSC Bend Sensitivity | |
| 19 | Square/Draw Level | |
| 20 | Saw Level | |
| 21 | Sub Oscillator Level | |
| 22 | Sub Octave Type | 1-3: -2oct asym / -2oct sym / -1oct sym |
| 23 | Noise Level | When riser mode is off |
| 24 | Filter Envelope Depth | |
| 25 | Filter LFO Depth | |
| 26 | Filter Keyboard Follow | |
| 27 | Filter Bend Sensitivity | |
| 28 | Amp Envelope Mode | 0=Gate, 1=Envelope |
| 29 | Envelope Trigger Mode | 0=LFO, 1=Gate, 2=Gate+Trig |
| 30 | Envelope Sustain | |
| 31 | Portamento Mode | 0=Off, 1=On, 2=Auto |
| 64 | Damper Pedal (Sustain) | |
| 65 | Portamento On/Off | |
| 71 | Filter Resonance | |
| 72 | Envelope Release | |
| 73 | Envelope Attack | |
| 74 | Filter Frequency (Cutoff) | |
| 75 | Envelope Decay | |
| 76 | Fine Tune | |
| 77 | Transpose Switch | |
| 78 | Noise Mode | 0=Pink, 1=White |
| 79 | LFO Mode | 0=Normal, 1=Fast |
| 80 | Poly Mode | 0=Mono, 1=Unison, 2=Poly, 3=Chord |
| 81-83 | Chord Voice 2-4 Switch | |
| 85-87 | Chord Voice 2-4 Key Shift | |
| 89 | Reverb Time | |
| 90 | Delay Time | |
| 91 | Reverb Level | |
| 92 | Delay Level | |
| 93 | Chorus Type | 0=Off, 1-4=Type 1-4 |
| 102 | OSC Draw Multiply | |
| 103 | OSC Chop Overtone | |
| 104 | OSC Chop Comb | |
| 105 | LFO Key Trigger | 0=Off, 1=On |
| 106 | LFO Sync | 0=Off, 1=On |
| 107 | OSC Draw Switch | 0=Off, 1=Step, 2=Slope |

### Other Messages
- **Note On/Off**: TX/RX, note range 0-127, velocity supported
- **Pitch Bend**: RX only
- **Program Change**: TX/RX, range 0-63 (64 patterns)
- **SysEx**: Not supported
- **Aftertouch**: Not supported

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
- No SysEx (no patch dump/restore)
- No aftertouch
- No Song Position Pointer / Song Select / Continue
- OSC Draw Form steps and OSC Chop step masks are not CC-addressable
- Arpeggiator configuration not available via MIDI
- Motion recording supports CC and pitch bend only in real-time recording mode, max 8 parameters per pattern

## Detailed Docs
- [MIDI Implementation (full CC reference)](midi-implementation.md)
- [Owner's Manual](pdfs/S-1-manual.md)
