# Arturia KeyStep MK II

## Overview
- **Type**: MIDI controller keyboard with sequencer and arpeggiator
- **Keys**: 37 slim keys, velocity-sensitive + aftertouch
- **Sequencer**: Polyphonic, 64 steps, 8 sequences
- **Arpeggiator**: 8 modes (up, down, inclusive, exclusive, random, order, poly, walk)
- **Chord mode**: Strum and chord memory

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | MIDI + power |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| CV Pitch | Out | 3.5mm (V/oct or Hz/V) |
| CV Gate | Out | 3.5mm (V-trig or S-trig) |
| CV Mod | Out | 3.5mm |
| Clock Out | Out | 3.5mm |
| Clock In | In | 3.5mm |
| Sustain | In | 1/4" pedal |
| Expression | In | 1/4" TRS |

## MIDI Implementation

### Channel
- Default: Channel 1 (configurable 1-16)
- Separate channel for arp/seq output vs keyboard

### Key CCs
| CC | Parameter |
|----|-----------|
| 1 | Mod wheel |
| 2 | Breath (assignable) |
| 11 | Expression pedal |
| 64 | Sustain pedal |

### Arpeggiator Modes
| Mode | Behavior |
|------|----------|
| Up | Low to high |
| Down | High to low |
| Inclusive | Up then down (endpoints played twice) |
| Exclusive | Up then down (endpoints played once) |
| Random | Random order |
| Order | Order played |
| Poly | All notes together |
| Walk | Random walk (step up or down) |

### Additional Features
- **Hold**: Latch notes for arp/seq
- **Tie**: Connect steps in sequencer
- **Rest**: Silent steps in sequencer
- **Swing**: Adjustable per sequence
- **Gate length**: Adjustable per sequence
- **Time division**: 1/4 to 1/32 including triplets

### Sync
- MIDI clock send/receive
- Analog clock in/out (configurable ppqn: 1, 2, 24, 48 ppqn)
- USB clock

## Software
- **Arturia MIDI Control Center** — MIDI mapping, firmware updates, sequence backup

## Limitations
- No sound engine (controller only)
- No screen/display
- Slim keys (not full-size)
- No MPE support
- Single MIDI DIN output (no thru)
