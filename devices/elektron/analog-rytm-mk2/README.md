# Elektron Analog Rytm MKII

## Overview
- **Type**: Analog drum machine / sampler
- **Voices**: 12 analog voices (8 drum tracks + 4 from performance pads)
- **Tracks**: 12 tracks (8 main + 1 FX + 1 CV)
- **Pads**: 12 velocity/pressure-sensitive rubber pads
- **Sequencer**: 64 steps per pattern, parameter locks, micro-timing
- **Sampling**: Mono sampling via audio input, +Drive storage (1GB)
- **Synthesis**: Analog drum engines + sample layering per voice
- **Effects**: Analog distortion + digital delay/reverb (send FX)
- **Display**: OLED screen

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI, Overbridge audio/control |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| MIDI DIN Thru | Thru | 5-pin DIN |
| Audio Out (main) | Out | 2x 1/4" TRS (L/R balanced) |
| Audio Out (individual) | Out | 8x 1/4" (one per track) |
| Headphone | Out | 1/4" stereo |
| Audio In | In | 2x 1/4" (L/R, for sampling + external processing) |
| Expression / CV | In | 1/4" (configurable) |

## MIDI Implementation

### Channel
- Default: Auto channel (configurable)
- Per-track channels assignable (tracks 1-12 can each have their own channel)

### Track MIDI Mapping
| Track | Default | Sound |
|-------|---------|-------|
| 1 | Ch 1 | Bass Drum |
| 2 | Ch 2 | Snare |
| 3 | Ch 3 | Rimshot/Clap |
| 4 | Ch 4 | Closed Hat |
| 5 | Ch 5 | Open Hat |
| 6 | Ch 6 | Cymbal |
| 7 | Ch 7 | Cowbell/Tom |
| 8 | Ch 8 | Tom/Percussion |
| FX | Ch 9 | Send FX |

### Key CCs
Extensive CC map — every synth parameter is automatable via CC:

| CC | Parameter |
|----|-----------|
| 3 | Synth Param 1 (Tune) |
| 4 | Synth Param 2 |
| 5 | Synth Param 3 |
| 6 | Synth Param 4 |
| 7 | Level |
| 8 | Synth Param 5 |
| 9 | Synth Param 6 |
| 10 | Pan |
| 16 | Sample Tune |
| 17 | Sample Fine Tune |
| 18 | Sample Bit Reduction |
| 19 | Sample Slot |
| 20 | Sample Start |
| 21 | Sample End |
| 22 | Sample Loop |
| 23 | Sample Level |
| 24 | Filter Frequency |
| 25 | Filter Resonance |
| 31 | Filter Type |
| 40 | Amp Attack |
| 41 | Amp Hold |
| 42 | Amp Decay |
| 43 | Amp Overdrive |
| 46 | Delay Send |
| 47 | Reverb Send |

### Drum Engines (per track)
Each track can load one of these analog engines:
- BD Hard, BD Classic, BD FM, BD Plastic, BD Silky, BD Sharp
- SD Hard, SD Classic, SD FM, SD Natural
- RS Hard, RS Classic, CP Classic
- BT Classic, CH Classic, OH Classic, CY Classic, CB Classic
- XT Classic, CH Metallic, OH Metallic, CY Metallic, CB Metallic

### Program Change
- 128 patterns per bank, 4 banks (A-D) = 512 patterns
- 128 kits
- Patterns and kits selectable via PC

### Overbridge
USB audio interface mode via Elektron's Overbridge software:
- Multi-channel audio (each track as separate audio stream)
- Full parameter control from DAW
- Sample transfer
- Total recall (save/restore full state)

### Sync
- MIDI clock send/receive
- DIN sync compatible (with adapter)
- Overbridge sync to DAW

## Unique Features
- **Parameter locks (p-locks)**: per-step parameter automation — any knob value can be different on each step
- **Micro-timing**: nudge individual steps off-grid (forward or back)
- **Conditional trigs**: steps that fire based on conditions (1st play, fill, probability %, A:B patterns)
- **Performance mode**: 12 pads mapped to performance macros (scene morphing)
- **Scenes**: A/B scene morphing via crossfader
- **Retrig**: per-step note repeat with configurable rate and velocity curve
- **Sound pool**: per-pattern sound assignments (non-destructive kit modifications)

## Software
- **Elektron Transfer** — sample management, project backup
- **Overbridge** — multi-channel USB audio, DAW integration, parameter control
- **Elektron OS** — firmware updates via USB

## Limitations
- Mono sampling only (no stereo sample recording)
- 12-voice limit (voices shared across all tracks)
- No time-stretching for samples
- Overbridge requires dedicated software (not class-compliant USB audio)
- Individual outputs are unbalanced 1/4" (main outs are balanced)
- No CV output (has CV/expression input only)
