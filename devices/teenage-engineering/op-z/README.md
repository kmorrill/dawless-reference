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
Each track maps to a MIDI channel (1-16, reconfigurable via OP-Z app or `midi.json` in content mode):

| Tracks | Type | Default Channels |
|--------|------|-----------------|
| 1-4 | Drum (kick, snare, perc, sample) | 1-4 |
| 5-8 | Synth (bass, lead, arp, chord) | 5-8 |
| 9-10 | FX (FX1, FX2) | 9-10 |
| 11-12 | Tape, Master | 11-12 |
| 13 | Perform | 13 |
| 14 | Lights (DMX via USB-C to USB-DMX adapter) | 14 |
| 15 | Photomatic/Motion (visuals via app) | 15 |
| 16 | Module/MIDI | 16 |

### Key CCs (defaults, per-track)
| CC | Function |
|----|----------|
| 1-2 | Parameter 1-2 (green/blue encoders) |
| 3 | Filter cutoff |
| 4 | Filter resonance |
| 5-8 | Envelope ADSR (attack, decay, sustain, release) |
| 9-12 | LFO (depth, speed, target, shape) |
| 13-14 | FX 1/FX 2 send |
| 16 | Volume |
| 17 | Portamento |
| 18 | Note style |

All CC numbers are customizable per-track via the OP-Z app or `midi.json`.

### Step Components
Step components (punch-in effects) add micro-timing, note repeat, and parameter sweeps per step. Sequenced on-device, not directly controllable via MIDI CC.

### Parameter Locks
Per-step parameter automation. Stored in the sequence, not transmitted as CC during playback unless configured.

### Program Change
Supported for pattern/preset selection.

### Multimedia Tracks
- **Lights** (track 14): outputs DMX via USB-C to USB-DMX converter (ENTTEC recommended)
- **Photomatic** (track 15): sequences stop-motion photo slideshows synced to music (via iOS/Mac app)
- **Motion** (track 15): sequences 3D Unity-based visuals (via iOS/Mac app). Shares track with Photomatic.
- All multimedia tracks use the same step sequencer and parameter locks as audio tracks.

## Sync
- USB MIDI clock (send/receive)
- Can sync with other TE devices via USB-C daisy chain

## Limitations
- No screen — requires app for detailed editing
- Limited external MIDI control compared to OP-XY
- Step component effects are internal only
- 16 steps per pattern (expandable with pattern chaining)
