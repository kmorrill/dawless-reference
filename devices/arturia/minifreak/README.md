# Arturia MiniFreak

## Overview
- **Type**: Hybrid polyphonic synthesizer
- **Voices**: 6 (true polyphony, per-voice analog filter) — paraphonic mode yields 12 voices sharing 6 filters
- **Oscillators**: 2 digital oscillator engines (22+ types from Arturia, Mutable Instruments/Plaits, Noise Engineering) + analog filter per voice
- **Keyboard**: 37 slim keys, velocity + channel aftertouch
- **Sequencer**: 64 steps (4 pages of 16), real-time/step recording, ratcheting, parameter automation
- **Arpeggiator**: Built-in with multiple modes
- **Effects**: 3 FX slots in series (distortion, bit crusher, wave folder, EQ, compressor, chorus, phaser, flanger, delay, reverb)
- **Modulation**: 7 sources x 13 destinations
- **Presets**: 384 factory + 256 user slots

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI + MiniFreak V plugin sync (class-compliant) |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| MIDI DIN Thru | Thru | 5-pin DIN |
| Audio Out | Out | 2x 1/4" (L/R) |
| Headphone | Out | 1/4" stereo |
| Clock In | In | 3.5mm |
| Clock Out | Out | 3.5mm |
| Expression | In | 1/4" TRS |

## MIDI Implementation

### Channel
- Default: Channel 1 (configurable 1-16)
- MPE: Not supported

### CC Map
| CC | Parameter |
|----|-----------|
| 1 | Mod Wheel |
| 5 | Glide |
| 14 | Osc 1 Wave |
| 15 | Osc 1 Timbre |
| 16 | Osc 1 Shape |
| 17 | Osc 1 Volume |
| 18 | Osc 2 Wave |
| 19 | Osc 2 Timbre |
| 20 | Osc 2 Shape |
| 21 | Osc 2 Volume |
| 22 | FX1 Time |
| 23 | FX1 Intensity |
| 24 | VCF Env Amount |
| 25 | FX1 Amount |
| 26 | FX2 Time |
| 27 | FX2 Intensity |
| 28 | FX2 Amount |
| 29 | FX3 Time |
| 30 | FX3 Intensity |
| 31 | FX3 Amount |
| 64 | Sustain Pedal |
| 68 | Cycling Env Rise Shape |
| 69 | Cycling Env Fall Shape |
| 70 | Osc 1 Tune |
| 71 | Filter Resonance |
| 73 | Osc 2 Tune |
| 74 | Filter Cutoff |
| 76 | Cycling Env Rise |
| 77 | Cycling Env Fall |
| 78 | Cycling Env Hold |
| 80 | Amp Env Attack |
| 81 | Amp Env Decay |
| 82 | Amp Env Sustain |
| 83 | Amp Env Release |
| 85 | LFO1 Rate |
| 87 | LFO2 Rate |
| 94 | Velocity Env Mod |
| 115 | Seq Gate |
| 116 | Seq Spice |
| 117 | Macro 1 |
| 118 | Macro 2 |

### Program Change
- CC#0 (Bank Select MSB): 0 = Bank 1, 1 = Bank 2, 2 = Bank 3
- Then PC 0-127 to select preset within bank
- 384 factory + 256 user slots

### Oscillator Engine Origins
| Source | Engines |
|--------|---------|
| Arturia | Basic Waves, SuperWave, Karplus-Strong, Wavetable, Granular, Sample, Audio In |
| Mutable Instruments (Plaits) | VAnalog, Waveshaper, Two-Op FM, Formant, Speech, Modal, Chord (osc 2 only) |
| Noise Engineering | Bass (wave-folding), SawX (supersaw + chorus/phase mod), Harm (wavefolding + additive) |

### Sync
- MIDI clock send/receive
- Analog clock in/out (3.5mm, configurable ppqn)

## Software
- **Arturia MIDI Control Center** — MIDI mapping, firmware updates
- **MiniFreak V** — software version of the synth (included), also serves as patch editor and preset backup tool

## Physical Controls

### Oscillator 1 Section
| Control | Type | CC | SHIFT Function |
|---------|------|-----|----------------|
| Type 1 | Encoder | -- | -- |
| Wave 1 | Knob | 14 | -- |
| Timbre 1 | Knob | 15 | -- |
| Shape 1 | Knob | 16 | -- |
| Volume 1 | Knob | 17 | Osc Mix |
| Tune 1 | Knob | 70 | Octave select |

### Oscillator 2 Section
| Control | Type | CC | SHIFT Function |
|---------|------|-----|----------------|
| Type 2 | Encoder | -- | -- |
| Wave 2 | Knob | 18 | -- |
| Timbre 2 | Knob | 19 | -- |
| Shape 2 | Knob | 20 | -- |
| Volume 2 | Knob | 21 | Osc Mix |
| Tune 2 | Knob | 73 | Octave select |

### Analog Filter
| Control | CC |
|---------|-----|
| Cutoff | 74 |
| Resonance | 71 |
| VCF ENV Amount | 24 |

### Main Envelope (ADSR)
| Control | CC |
|---------|-----|
| Attack | 80 |
| Decay | 81 |
| Sustain | 82 |
| Release | 83 |

### Cycling Envelope
| Control | CC | SHIFT (CC) |
|---------|-----|------------|
| Rise | 76 | Rise Shape (68) |
| Fall | 77 | Fall Shape (69) |
| Hold | 78 | -- |

### LFO Section
| Control | CC |
|---------|-----|
| LFO1 Rate | 85 |
| LFO2 Rate | 87 |

### Effects (3 slots)
| Control | FX1 CC | FX2 CC | FX3 CC |
|---------|--------|--------|--------|
| Time | 22 | 26 | 29 |
| Intensity | 23 | 27 | 30 |
| Amount | 25 | 28 | 31 |

### Performance Controls
| Control | Type | CC |
|---------|------|-----|
| Touch Strip 1 | Touch strip | 117 (Macro 1) |
| Touch Strip 2 | Touch strip | 118 (Macro 2) |
| Glide | Knob | 5 |
| Velocity Env Mod | -- | 94 |
| SEQ Gate | -- | 115 |
| SEQ Spice | -- | 116 |

**Note**: Oscillator Type selection is NOT available via CC (requires program change or preset editor). 41 CCs total.

## 3rd-Party Repos

| Repo | Description |
|------|-------------|
| [pencilresearch/midi](https://github.com/pencilresearch/midi) | Complete MiniFreak MIDI CC map in CSV format |
| [tgerla/minifreak-patches](https://github.com/tgerla/minifreak-patches) | Custom patches for MiniFreak and MiniFreak V |
| [midi.guide](https://midi.guide/) | Open CC/NRPN database with MiniFreak entry |

## Limitations
- No MPE support
- USB audio is stereo only (not multi-channel)
- Sequencer limited to 64 steps
- No SysEx preset dump (backup via MiniFreak V software only)
- Expression pedal input is single (no second pedal input)
