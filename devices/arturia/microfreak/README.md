# Arturia MicroFreak

## Overview
- **Type**: Hybrid digital synthesizer
- **Voices**: 4 (paraphonic — single analog filter shared across voices, digital VCAs per-voice)
- **Oscillators**: 1 digital oscillator with 22 engine types (Arturia, Mutable Instruments/Plaits, Noise Engineering)
- **Filter**: Analog state-variable 12dB/oct (LP/BP/HP)
- **Keyboard**: 25-key capacitive touch (no moving keys), polyphonic aftertouch
- **Sequencer**: 64 steps, 2 patterns per preset (A/B banks)
- **Arpeggiator**: Up, Order, Random, Pattern modes + Spice & Dice
- **Modulation**: 5 sources x 7 destinations
- **Presets**: 320 factory + 128+ user slots

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI + power (class-compliant, can bus-power unit) |
| 3.5mm TRS MIDI Out | Out | Type A (adapter included) |
| 3.5mm TRS MIDI In | In | Type A (adapter included) |
| Audio Out | Out | 1x 1/4" mono |
| Headphone | Out | 1/8" stereo |
| Clock In | In | 3.5mm |
| Clock Out | Out | 3.5mm |
| CV Pitch | Out | 3.5mm (1V/oct) |
| CV Gate | Out | 3.5mm |
| CV Pressure | Out | 3.5mm (routes aftertouch to modular) |

No full-size 5-pin DIN MIDI (3.5mm TRS adapters included).

## MIDI Implementation

### Channel
- Default: Channel 1 (configurable 1-16)
- Not MPE — single channel, but sends polyphonic aftertouch (MIDI 0xAn)

### CC Map
| CC | Parameter |
|----|-----------|
| 2 | Keyboard Spice |
| 5 | Glide |
| 9 | Oscillator Type |
| 10 | Oscillator Wave |
| 12 | Oscillator Timbre |
| 13 | Oscillator Shape |
| 23 | Filter Cutoff |
| 24 | Cycling Env Amount |
| 26 | Envelope Filter Amount |
| 28 | Cycling Env Hold |
| 29 | Envelope Sustain |
| 64 | Hold Button (toggle) |
| 83 | Filter Resonance |
| 91 | Arp/Seq Rate (free) |
| 92 | Arp/Seq Rate (sync) |
| 93 | LFO Rate (free) |
| 94 | LFO Rate (sync) |
| 102 | Cycling Env Rise |
| 103 | Cycling Env Fall |
| 105 | Envelope Attack |
| 106 | Envelope Decay |

### Oscillator Engine Types (22 as of firmware 5.0)
| Source | Engines |
|--------|---------|
| Arturia (12) | Basic Waves, SuperWave, Harmonic, Karplus-Strong, Wavetable, Noise, Vocoder, User Wavetable, Sample, Scan Grain, Cloud Grain, Hit Grain |
| Mutable Instruments (7) | Virtual Analog, Waveshaper, Two-Op FM, Formant, Chords, Speech, Modal |
| Noise Engineering (3) | Bass (wave-folding), SawX (supersaw + chorus/phase mod), Harm (wavefolding + additive) |

### Program Change
- Supports PC for preset selection

### Sync
- MIDI clock send/receive
- Analog clock in/out (3.5mm, configurable ppqn)

## Unique Features
- **Capacitive keyboard**: touch surface detects finger position and polyphonic pressure. Enables slides and gestures.
- **Spice & Dice**: Spice controls gate probability, Dice shuffles note order in sequences.
- **Polyphonic aftertouch**: each key independently senses pressure (usable as controller for external poly-AT synths).
- **CV outputs**: Pitch, Gate, and Pressure CV for connecting to modular gear.

## Software
- **Arturia MIDI Control Center** — patch management, MIDI mapping, firmware updates

## MicroFreak vs MiniFreak
| Feature | MicroFreak | MiniFreak |
|---------|-----------|-----------|
| Voices | 4 paraphonic | 6 true polyphonic |
| Oscillators | 1 engine | 2 engines |
| Filter | 1 shared analog SVF | Per-voice analog filter |
| Effects | None built-in | 3 FX slots |
| Keyboard | 25-key capacitive, poly AT | 37-key slim keys, channel AT |
| MIDI | USB + 3.5mm TRS | USB + full-size 5-pin DIN |
| Audio out | Mono | Stereo |
| Mod Matrix | 5x7 | 7x13 |

## Limitations
- Mono audio output only (no stereo)
- Paraphonic, not truly polyphonic (shared filter)
- No full-size DIN MIDI (3.5mm TRS only)
- No built-in effects
- No velocity (pressure-sensitive but no strike velocity)
- 4-voice maximum
