# Polyend Synth

## Overview
- **Type**: Multi-engine polyphonic desktop synthesizer
- **Voices**: 8 (shared across 3 simultaneous synth engine slots)
- **Multitimbral**: 3 parts (synth slots), each loadable with any engine
- **Pads**: 60 (5x12 grid), silicone, velocity-sensitive + polyphonic aftertouch
- **Display**: Color LCD
- **Engines (9+1)**: ACD (acid/303), FAT (Minimoog), VAP (Prophet poly), WAVS (wavetable), WTFM (2-op FM), PMD (physical modeling), PHZ (phase distortion), GRAIN (granular, paraphonic), DWA (dual wavetable, fw 1.3+), MIDI Instrument (CC controller preset)
- **Dimensions**: 282 x 207 x 35 mm, 1.2 kg
- **Power**: 5V / 1A via USB-C (can run from battery bank)
- **Internal clock**: 96 PPQN internal, 24 PPQN over MIDI
- **Tempo**: 10-400 BPM

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | Power + USB MIDI + USB storage mode |
| 3.5mm TRS MIDI Out | Out | Type B (5-pin DIN adapter included) |
| 3.5mm TRS MIDI In | In | Type B (only 1 adapter included — need 2nd for simultaneous in/out) |
| 3.5mm stereo TRS | Out | Shared line + headphone output (9 dBu peak line, SNR 97-98 dB) |
| microSD | — | 16 GB included; firmware, patches, samples, scenes |

No CV/gate, no expression pedal, no individual per-part outputs, no USB audio.

## MIDI Channel Configuration
- Each of the 3 synth slots has an assignable MIDI channel
- Same channel is used for both send and receive (can create feedback loops)
- No dedicated per-synth independent I/O channels
- No MIDI Thru (input does not pass to output)

## MIDI CC Map

### Common Across All Engines
| CC | Parameter |
|----|-----------|
| 74 | Filter Cutoff |
| 71 | Filter Resonance |
| 77 | Filter Env Amount |
| 78 | Filter LFO |
| 75 | Amp Attack |
| 72 | Amp Decay |
| 76 | Amp Sustain |
| 73 | Amp Release |
| 80 | Filter Env Attack |
| 81 | Filter Env Decay |
| 82 | Filter Env Sustain |
| 83 | Filter Env Release |
| 46 | Aux Env Attack |
| 47 | Aux Env Decay |
| 48 | Aux Env Sustain |
| 49 | Aux Env Release |
| 54 | LFO Frequency |

### Engine-Specific Oscillator Parameters (CC 20-31)
These vary per engine — CC 20-31 map to engine-specific oscillator/waveshaping parameters. See the full per-engine CC tables in the Polyend Synth manual.

## Program Change
- 8 slots (PC 0-7), mapped at the scene level
- Limited to scene-level recall, not individual preset selection

## Sync
- MIDI clock send/receive (24 PPQN over MIDI, 96 PPQN internal)
- Transport start/stop
- Tempo range 10-400 BPM

## Physical Controls

### Top Panel (87 total controls)
| Control | Type | Function |
|---------|------|----------|
| Volume | Touch encoder | Master volume |
| Tempo | Touch encoder | Global tempo; Shift = swing 25%-75% |
| Pitchbend | Touch encoder | Pitch bend -100 to +100, spring-returns to 0 on release |
| C1, C2, C3 | Touch encoders (x3) | Macro / combo controls for one or all synths |
| Parameter knobs | Touch encoders (3x3 grid, 9 total) | Context-dependent: edit whichever page is displayed |
| Screen encoder | Clickable rotary (detented) | Navigate menus, select options |
| Screen buttons (x3) | Buttons below display | Select synth 1/2/3 or context actions |
| Engine | Button | Open oscillator/engine page |
| Filter | Button | Open filter page; Shift = cycle effects |
| ADSR | Button | Open filter/amp envelope pages |
| LFO | Button | Open LFO page |
| Seq | Button | Cycle Off / Arp / Sequencer; Shift = modulation matrix |
| Mixer/Effects | Button | Open mixer; Shift = effects |
| Scene/Preset | Button | Scene manager; Shift = preset browser |
| Shift | Modifier | Access secondary functions |
| 60 Pads | 5x12 RGB backlit silicone | Play notes, velocity + poly aftertouch |

## Arpeggiator
14 types: Up, Down, Play Order, Random, Chord, Dyad, Triad, Inside Out, Outside In, Up Down, Down Up, Weave, Return, Double Return. 1-8 octave range. Swing 50-75%, 11 groove templates, humanize 0-100%.

## Sequencer
- Up to 64 notes per synth engine (3 independent sequencers)
- Real-time recording only (no step-input editing)
- Pad transposes playback relative to recorded pattern
- No parameter locks, no per-step editing, no pattern chaining, no song mode

## SysEx
None. No SysEx or NRPN support.

## Software
- **Firmware (.psf)**: Downloaded from polyend.com, manually copied to microSD
- **USB Storage Mode**: Access microSD contents via USB-C for file transfer
- No companion app, no desktop editor, no preset manager, no librarian

Latest firmware: **1.3.1** (added DWA engine, MIDI pad mapping, MIDI Instrument, customizable pad colors, aftertouch type selection).

## 3rd-Party Repos
No Polyend Synth-specific repos exist on GitHub. The broader Polyend ecosystem is focused on the Tracker product line.

## Not Controllable via MIDI
- Mixer levels and panning
- Effect send amounts and parameters
- Scene loading (no MIDI scene recall)
- Grid layout/mode changes
- Macro assignments

## Limitations
- **8 voices shared** across 3 parts (no per-part voice allocation)
- Single stereo output (shared line + headphone on one 3.5mm jack)
- No USB audio interface
- No individual per-part outputs
- No SysEx, no NRPN
- No MIDI Thru
- Same channel for send/receive per synth (feedback risk)
- Program Change limited to 8 slots
- Type B TRS MIDI (many devices use Type A)
- Only 1 MIDI DIN adapter included
- Generic knobs change function per engine — requires constant screen reference
- Pad velocity can be inconsistent
- CPU overload possible when stacking heavy engines (especially GRAIN)
- Prone to digital clipping when layering 3 engines at high mixer levels
