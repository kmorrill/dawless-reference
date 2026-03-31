# Arturia MicroFreak

## Overview
- **Type**: Hybrid digital synthesizer
- **Voices**: 4 (paraphonic)
- **Oscillators**: 1 digital oscillator (13 types including wavetable, Karplus-Strong, harmonic, superwave, FM, granular, etc.)
- **Filter**: Analog Oberheim SEM-style state-variable filter (LP/BP/HP)
- **Keyboard**: 25-key capacitive touch (no moving keys), pressure-sensitive
- **Sequencer**: 4 tracks, 64 steps
- **Arpeggiator**: Built-in with multiple modes
- **Modulation**: 3-slot matrix (cycling envelope, LFO, pressure/keyboard as sources)

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI + firmware |
| MIDI DIN Out | Out | 5-pin DIN (TRS adapter included) |
| Audio Out | Out | 1x 1/4" mono |
| Headphone | Out | 1/8" stereo |
| Clock In | In | 3.5mm |
| Clock Out | Out | 3.5mm |
| CV/Gate | Out | 2x 3.5mm (1V/oct pitch + gate) |
| Pressure Out | Out | 3.5mm CV |

## MIDI Implementation

### Channel
- Default: Channel 1 (configurable 1-16)
- MPE: Not supported (paraphonic only)

### Key CCs
| CC | Parameter |
|----|-----------|
| 1 | Mod wheel (external) |
| 9 | Osc Type |
| 10 | Osc Wave |
| 11 | Osc Timbre |
| 12 | Osc Shape |
| 23 | Filter Cutoff |
| 24 | Filter Resonance |
| 26 | Cycling Env Rise |
| 27 | Cycling Env Fall |
| 28 | Cycling Env Hold |
| 29 | LFO Rate |
| 33 | Glide |
| 85 | Spice |
| 86 | Dice |

Full CC map available via Arturia MIDI Control Center.

### Program Change
- 256 preset slots (128 factory + 128 user)
- Bank select (CC 0) + Program Change

### Sync
- MIDI clock send/receive
- Analog clock in/out (3.5mm, configurable ppqn)

## Unique Features
- **Capacitive keyboard**: no mechanical keys — touch surface detects finger position and pressure. Enables slides and gestures not possible on standard keys.
- **Spice & Dice**: randomization controls that add variation to sequences. Spice = amount of variation, Dice = re-roll the randomization.
- **Paraphonic voicing**: 4 voices share a single filter, so all notes have the same filter character.
- **Vocoder**: built-in vocoder mode using the line input.

## Software
- **Arturia MIDI Control Center** — patch management, MIDI mapping, firmware updates

## Limitations
- Mono audio output only (no stereo)
- Paraphonic, not truly polyphonic (shared filter)
- No MIDI input on DIN (output only, input via USB only)
- No velocity (capacitive keyboard has pressure/aftertouch but not strike velocity)
- 4-voice maximum
