# Conductive Labs NDLR

## Overview
- **Type**: MIDI harmonizer / arpeggiator ("harmonic conductor")
- **Parts**: 4 (Drone, Pad, Motif 1, Motif 2)
- **Concept**: Set key + mode + degree, all parts generate harmonically coherent output
- **Sequencer**: Chord sequencer for progressions

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| MIDI DIN Out 1 | Out | Configurable per part |
| MIDI DIN Out 2 | Out | Configurable per part |
| MIDI DIN Thru | Thru | Pass-through |
| USB | Out | MIDI over USB |

## Core Concept

The NDLR is a **harmonic conductor** — you steer musical behaviors (not individual notes) across synchronized parts under a shared harmonic context:

- **Key** (C through B)
- **Mode** (16 modes: Ionian through Phrygian Dominant)
- **Degree** (I through VII)
- **Chord Type** (triad, 7th, sus2, sus4, etc.)

Each part generates notes from the **legal note list** derived from the current harmonic context.

### Parts
| Part | Role | Behavior |
|------|------|----------|
| Drone | Foundation | Sustained root/5th/octave |
| Pad | Harmony/texture | Chord voicings with spread and strum |
| Motif 1 | Melodic motion | Pattern + rhythm through legal notes |
| Motif 2 | Melodic motion | Independent pattern + rhythm |

## MIDI Implementation
- Each part assignable to any MIDI channel (1-16)
- Each part routable to either DIN output or USB
- CC control for real-time parameter changes
- Full CC table in [ndlr-manual.md](ndlr-manual.md)

## Design Philosophy
See [vision-brief.md](vision-brief.md) for the harmonic conductor concept.

## Full Manual
See [ndlr-manual.md](ndlr-manual.md) — complete v1.7 user manual.
