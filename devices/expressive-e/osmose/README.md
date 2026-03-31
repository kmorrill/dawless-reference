# Expressive E Osmose

## Overview
- **Type**: MPE polyphonic synthesizer + expressive MIDI controller
- **Keys**: 49 (original) / 61 (newer model)
- **Polyphony**: 24 voices
- **Engine**: Haken EaganMatrix (same as Continuum)
- **Expression**: Per-note 3-axis expression (initial strike, pressure, pitch bend per key)

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI (multiple ports) |
| MIDI DIN Out | Out | 5-pin DIN |
| MIDI DIN In | In | 5-pin DIN |
| Audio Out | Out | 2x 1/4" (L/R) |
| Headphone | Out | 1/4" stereo |
| Pedal 1 | In | Expression/sustain |
| Pedal 2 | In | Expression/sustain |

## MPE Implementation

### Expression Dimensions
| Dimension | MIDI Message | Description |
|-----------|-------------|-------------|
| Strike | Note-on velocity | Initial key press force |
| Pressure | Channel aftertouch (per-note) | Continuous downward force |
| Slide (Y) | CC 74 | Forward/backward position on key |
| Pitch (X) | Pitch bend (per-channel) | Left/right key movement |
| Lift | Note-off velocity | Release force |

### MPE Zone Setup
- **Lower Zone**: Channel 1 = manager, Channels 2-16 = member channels (15 voices)
- Pitch bend range: configurable (default 48 semitones for full expression range)
- Compatible with any MPE-capable synth/plugin

### MIDI Profiles
| Profile | Behavior |
|---------|----------|
| MPE | Full per-note expression on member channels |
| MPE+ | Extended expression (Osmose-specific, for EaganMatrix) |
| Poly Aftertouch | Per-note pressure without MPE channel allocation |
| Classic Keyboard | Velocity + channel aftertouch only, no MPE |

## Internal Synth (EaganMatrix)
- Haken Audio's EaganMatrix — deep modular synthesis engine
- Hundreds of factory presets designed for expressive playing
- Preset editor via Haken Editor software (Mac/Win)
- All parameters respond to MPE expression data

## Limitations
- Internal synth presets not editable from front panel (requires Haken Editor)
- When used as MIDI controller for external gear, non-MPE synths only receive velocity + aftertouch
- DIN MIDI output limited to standard MIDI bandwidth (may bottleneck with heavy MPE data)
