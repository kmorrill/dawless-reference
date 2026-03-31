# Arturia KeyStep MK II

## Overview
- **Type**: MIDI controller keyboard with sequencer and arpeggiator
- **Keys**: 32 slim keys, velocity-sensitive + channel aftertouch (curves adjustable)
- **Sequencer**: Polyphonic, 64 steps, up to 8-note polyphony per step, 64 pattern slots
- **Arpeggiator**: 16 modes including Phrase Arp (melodic transformer)
- **Display**: OLED screen with push encoder
- **Power**: USB bus-powered (no wall adapter needed)
- **Dimensions**: 19.06" x 5.71" x 1.97", 2.43 lb

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | MIDI + power (class-compliant) |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| CV Pitch | Out | 3.5mm (1V/oct) |
| CV Gate | Out | 3.5mm |
| CV Mod 1 | Out | 3.5mm (assignable: velocity, aftertouch, mod wheel, etc.) |
| CV Mod 2 | Out | 3.5mm (assignable) |
| Clock Out | Out | 3.5mm |
| Clock In | In | 3.5mm |
| Sustain/Expression | In | 1/4" TRS |

All MIDI output simultaneous on USB + DIN.

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

### Arpeggiator (16 modes)
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
| + 8 more | Pattern variations and Phrase Arp |

**Phrase Arp**: remaps stored patterns to whatever notes you play — a melodic transformer that applies a rhythmic/melodic shape to your input.

### Sequencer Features
- Real-time, step, and unquantized recording
- Pattern chaining for song-like arrangements
- **Mutate**: introduces controlled random variations to existing patterns
- **Capture**: convert arpeggio output into a sequence
- **Spice & Dice**: randomize gate triggering and note order

### Chord Mode
- Trigger full chords from single keys
- Locks to a harmonic key for consistent voicings

### Scale Mode
- Constrains keyboard output to a selected scale
- Keys outside the scale are quantized or muted

### Sync
- MIDI clock send/receive
- Analog clock in/out (configurable: 1, 2, 24, 48 ppqn)
- Sends/receives start/stop messages
- USB clock

## Software
- **Arturia MIDI Control Center** — MIDI mapping, firmware updates, sequence backup

## Limitations
- No sound engine (controller only)
- Slim keys (not full-size)
- No MPE support
- Single MIDI DIN output (no thru)
- 32 keys (not 37)
