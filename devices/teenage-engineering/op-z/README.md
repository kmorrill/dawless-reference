# Teenage Engineering OP-Z

## Overview
- **Type**: 16-track sequencer / synthesizer
- **Tracks**: 16 (drum, synth, FX, tape, visual)
- **Sequencer**: 16 steps with parameter locks, step components
- **Polyphony**: Varies by engine
- **Screen**: None built-in (uses iOS/Android app for visual interface)

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | MIDI device mode, audio, charging |
| Bluetooth LE | Out | MIDI, connects to OP-Z app |
| 3.5mm line out | Out | Stereo audio |

No DIN MIDI. USB-C MIDI is class-compliant.

## MIDI Implementation

### Channels
Each track maps to a MIDI channel (1-16):

| Tracks | Type | Default Channels |
|--------|------|-----------------|
| 1-4 | Drum (kick, snare, perc, sample) | 1-4 |
| 5-8 | Synth (bass, lead, arp, chord) | 5-8 |
| 9-12 | FX/tape | 9-12 |
| 13-16 | Visual/motion | 13-16 |

### Key CCs
| CC | Function |
|----|----------|
| 1 | Parameter 1 (green encoder) |
| 2 | Parameter 2 (blue encoder) |
| 3 | Filter |
| 4 | Resonance |

### Step Components
Step components (punch-in effects) add micro-timing, note repeat, and parameter sweeps per step. These are sequenced on-device but not directly controllable via MIDI CC.

### Parameter Locks
Per-step parameter automation. Stored in the sequence, not transmitted as CC during playback unless configured.

## Sync
- USB MIDI clock (send/receive)
- Can sync with other TE devices via USB-C daisy chain

## Limitations
- No screen — requires app for detailed editing
- Limited external MIDI control compared to OP-XY
- Step component effects are internal only
- 16 steps per pattern (expandable with pattern chaining)
