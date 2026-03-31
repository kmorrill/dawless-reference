# Conductive Labs NDLR

## Overview
- **Type**: MIDI harmonizer / arpeggiator ("harmonic conductor")
- **Parts**: 4 (Drone, Pad, Motif 1, Motif 2)
- **Concept**: Set key + mode + degree, all parts generate harmonically coherent output
- **Presets**: 9 (1 factory + 8 user)
- **Patterns/Rhythms**: 20 preset + 20 user per slot
- **Chord Sequencer**: 5 slots
- **Display**: 1.8" 128x160 LCD
- **Controls**: 8 encoders, 7 chord buttons
- **No sound engine** — pure MIDI controller

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| MIDI DIN Out 1 | Out | Configurable per part |
| MIDI DIN Out 2 | Out | Configurable per part |
| MIDI DIN In | In | 5-pin DIN (Port B) |
| MIDI DIN Thru | Thru | Pass-through |
| USB | In/Out | MIDI over USB (4 virtual ports) |
| CV Clock In | In | 3.5mm |

## Core Concept

The NDLR is a **harmonic conductor** — you steer musical behaviors (not individual notes) across synchronized parts under a shared harmonic context:

- **Key** (C through B, circle of 5ths)
- **Mode** (16 modes: Major, Dorian, Phrygian, Lydian, Mixolydian, Minor, Locrian, Gypsy Minor, Harmonic Minor, Minor Pentatonic, Whole Tone, Tonic 2nds/3rds/4ths/6ths)
- **Degree** (I through VII)
- **Chord Type** (triad, 7th, sus2, alt1, alt2, sus4, 6th)

Each part generates notes from the **legal note list** derived from the current harmonic context.

### Parts
| Part | Role | Behavior |
|------|------|----------|
| Drone | Foundation | Sustained root/5th/octave with cadence triggers |
| Pad | Harmony/texture | Chord voicings with spread, strum, and quantized retrigger |
| Motif 1 | Melodic motion | Pattern + rhythm through legal notes |
| Motif 2 | Melodic motion | Independent pattern + rhythm |

## MIDI CC Map

All CCs received on the configurable **MIDI Control channel** (Settings 1 menu).

### Global / Harmonic Context
| CC | Parameter | Range |
|----|-----------|-------|
| 73 | Key | 1-12 (circle of 5ths: C, G, D, A, E, B, F#, Db, Ab, Eb, Bb, F) |
| 74 | Mode/Scale | 0-15 |
| 26 | Chord Degree | 1-7 |
| 27 | Chord Type | 1-7 (triad, 7th, sus2, alt1, alt2, sus4, 6th) |
| 69 | Chord Inversion | 0-62=on, 63-127=off |
| 72 | Tempo | 5-127 (BPM × 2, so 10-254 BPM) |
| 59 | Humanize | 0-10 |
| 89 | Load Chord Sequence | 1-5 |
| 57 | Black Keys Control | 0-127 |
| 68 | Clock-In Mode | 0-7 (Internal, 5-Pin A/B, USB 1-4, CV) |
| 58 | Send Start/Stop/Continue | 0-8 |

### Drone
| CC | Parameter | Range |
|----|-----------|-------|
| 21 | MIDI Channel | 1-16 |
| 20 | MIDI Port | 1-7 |
| 32 | Position (octave) | 2-6 |
| 33 | Type | 1-4 (Root, Root+Oct, Root+5th, Root+Oct+5th) |
| 34 | Trigger | 1-19 (1-8 key-root based, 9-19 chord based) |
| 86 | Pause/Play | 0-62=pause, 63-127=play |

### Pad
| CC | Parameter | Range |
|----|-----------|-------|
| 19 | MIDI Channel | 1-16 |
| 18 | MIDI Port | 1-7 |
| 28 | Position | 1-100 |
| 29 | Strum | 1-7 (None, 1/32, 1/16, 1/8T, 3+1/8T, 1/8, 3+1/8) |
| 30 | Range | 1-100 |
| 31 | Spread | 1-6 |
| 63 | Velocity | 1-127 |
| 67 | Poly Chain | 1-4 |
| 70 | Quantization | 1-3 (1/4, 1/8, none) |
| 85 | Pause/Play | 0-62=pause, 63-127=play |

### Motif 1
| CC | Parameter | Range |
|----|-----------|-------|
| 23 | MIDI Channel | 1-16 |
| 22 | MIDI Port | 1-7 |
| 35 | Position | 1-10 |
| 36 | Pattern Length | 1-16 |
| 37 | Variation | 1-6 (Fwd, Bwd, Ping-Pong, PP+repeats, Odd-Even, Random) |
| 38 | Pattern | 1-40 (1-20 internal, 21-40 user) |
| 39 | Clock Divide | 1-6 (1/1, 1/2, 1/4, 1/8, triplets 1/3 & 1/6) |
| 40 | Rhythm Length | 4-32 |
| 41 | Accent | 1-10 |
| 42 | Rhythm | 1-40 (1-20 internal, 21-40 user) |
| 64 | Velocity | 1-127 |
| 66 | Low Velocity | 1-100 (shared with Motif 2) |
| 87 | Pause/Play | 0-62=pause, 63-127=play |

### Motif 2
Mirrors Motif 1 with offset CCs: 25 (channel), 24 (port), 43-50 (params), 65 (velocity), 88 (pause/play). CC 66 is shared.

## Program Change
- **Receives** PC on the MIDI Control channel (any port)
- PC 0 = factory preset, PC 1-8 = user presets
- Does **not** send Program Change
- Beyond 9 presets requires USB serial interface (not MIDI)

## Sync
- Internal clock (10-254 BPM via CC 72)
- External MIDI clock from 5-Pin or USB
- CV clock input (3.5mm)
- Sends MIDI Start/Stop/Continue (configurable routing)
- Receives Start/Stop/Continue on Port B (5-pin) and USB(1) only

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [Barilium8/The-NDLR-Librarian](https://github.com/Barilium8/The-NDLR-Librarian) | USB serial preset save/load (by NDLR co-creator Darryl McGee) |
| [ratbag98/ndlr_control](https://github.com/ratbag98/ndlr_control) | Max 4 Live MIDI effects for NDLR control |
| [GeospatialDaryl/pyNDLR](https://github.com/GeospatialDaryl/pyNDLR) | Python serial control |

## Limitations
- **No sound engine** — pure MIDI controller, requires external synths
- **Motif Offset not CC-controllable** — front panel only
- **Tempo cannot be modulated** via the mod matrix
- **9 presets max** (1 factory + 8 user) — very limited storage
- **5 chord sequence slots** only
- **20+20 patterns/rhythms per slot** — small library
- **5 preset modulators (Mod 1-5) are fixed** — only the 3 LFOs are user-configurable
- **No time signature** — internally beat-based only (some 3/4 and 5/4 drone cadences exist but don't align with beat indicator)
- **Stuck notes** — known issue with synths that have poor MIDI implementations; Pad can play up to 22 notes simultaneously, saturating 5-pin MIDI at high strum rates
- **External clock dependency** — if clock source stops while notes are playing, notes get stuck
- **Tiny screen** (1.8" LCD) — deep menu diving required
- **No display of actual note names** being played
- **BPM via CC maxes at 254** (tap tempo 40-300)

## Design Philosophy
See [vision-brief.md](vision-brief.md) for the harmonic conductor concept.

## Full Manual
See [ndlr-manual.md](ndlr-manual.md) — complete v1.7 user manual.
