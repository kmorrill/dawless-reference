# Expressive E Osmose

## Overview
- **Type**: MPE polyphonic synthesizer + expressive MIDI controller
- **Keys**: 49 (original) / 61 (newer model), proprietary "Augmented Keyboard Action" (A.K.A.)
- **Polyphony**: 24 voices (often 12 stereo voices)
- **Engine**: Haken EaganMatrix (6 SHARC DSPs) — same as Continuum Fingerboard
- **Expression**: Per-note 3-axis expression (strike, pressure/slide, lateral pitch bend)
- **Presets**: 580 factory, up to 2,048 slots
- **Power**: External PSU 12V 1.5A (not USB-powered)
- **Dimensions (49-key)**: 895 x 310 x 90mm, 8.3kg

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B (Port 1 "USB Play") | In/Out | MIDI controller output to DAW/external gear |
| USB-B (Port 2 "USB Haken") | In/Out | EaganMatrix engine input; Haken Editor communication |
| MIDI DIN In | In | 5-pin DIN |
| MIDI DIN Out/Thru | Out | 5-pin DIN, configurable (keyboard out, DSP input, thru, or CVC) |
| Audio Out | Out | 2x 1/4" (L/R) |
| Headphone | Out | 1/4" stereo |
| Pedal 1 | In | 1/4" TRS, configurable (sustain or expression) |
| Pedal 2 | In | 1/4" TRS, configurable (sustain or expression) |

**Note**: USB presents as two virtual MIDI devices on the host. Avoid recording channels 15-16 from USB Haken port (Haken Editor communication data).

## Keybed Expression Axes
| Axis | Gesture | Range |
|------|---------|-------|
| Z (vertical) | Key press depth | ~9mm initial dip + extended aftertouch travel |
| X (lateral) | Key wiggle left/right | Blacks ±4mm, whites nearly as far when depressed |
| "Y" (emulated) | Deeper key press beyond initial travel | Derived from pressure depth, not true front-to-back slide |

**Important**: The Osmose has no true Y-axis (front-to-back) sensing. The "slide" dimension (CC 74) is derived from deeper key pressure, so pressure and slide are somewhat coupled.

## MPE Implementation

### Expression Dimensions
| Dimension | MIDI Message | Source Gesture |
|-----------|-------------|----------------|
| Strike | Note-on velocity | Initial key press speed |
| Glide (X) | Per-channel pitch bend | Lateral key wiggle (left/right) |
| Pressure | Per-channel channel pressure (aftertouch) | Key press depth (first range) |
| Slide/Timbre | Per-channel CC 74 | Key press depth (extended/deeper range) |
| Lift | Note-off velocity | Release speed |

### MPE Zone Setup
- **Lower Zone**: Channel 1 = global, Channels 2-16 = member channels (up to 15 voices)
- Member channel count is reducible via settings
- Pitch bend range: configurable (typically ±48 semitones; physical key travel is small, so bending fraction must be tuned to avoid overshoot)

### MIDI Output Profiles (4 modes)
| Profile | Channel(s) | Velocity | Aftertouch | Pitch Bend | Best For |
|---------|-----------|----------|------------|------------|----------|
| **MPE** (default) | Ch 1 global, Ch 2-16 per-note | Yes | Channel pressure per-channel + CC 74 | Per-channel | MPE-compatible synths/plugins |
| **Classic Keyboard** | Ch 1 only | Yes | Mono channel pressure | Disabled | Legacy MIDI gear |
| **Poly Aftertouch** | Ch 1 only | Yes | Polyphonic aftertouch | Disabled | Synths with poly AT (e.g., OB-6) |
| **Multi-Channel** | Ch 1-16 per-note, no global | Yes | Per-channel | Per-channel | Non-MPE gear that benefits from multi-channel |

In Multi-Channel mode, global messages (e.g., CC 1 from mod slider) are sent as copies on every active channel.

### MPE+
Proprietary Haken protocol extending MPE with higher time and magnitude resolution. Replaces traditional velocity with "attack trajectory" analysis. Available on USB Haken port only.

## Global Controls
| Control | MIDI Message |
|---------|-------------|
| Pitch slider | Pitch bend on global channel (Ch 1 in MPE) |
| Modulation slider | CC 1 on global channel |
| Pedal 1 | Configurable, typically CC 64 (sustain) |
| Pedal 2 | Configurable, typically expression (continuous) |

## Internal Synth (EaganMatrix)
- Haken Audio's EaganMatrix — deep modular synthesis engine on 6 SHARC DSPs
- Synthesis types: virtual analog, FM, physical modeling, granular, additive, convolution
- Architecture: 5 oscillators/waveshapers, noise sources, 5 multi-mode filters, 2 filter banks (8 or 48 filters), time delays, convolution module, recirculator
- Built-in effects: reverb, delay/echo, compressor/drive, shelving EQ
- All parameters respond natively to MPE expression data
- Preset editing via Haken Editor software (Mac/Win), not from front panel

## Limitations
- **No true Y-axis**: Slide (CC 74) derived from pressure depth, not finger position — pressure and slide are coupled
- **Monotimbral only**: Cannot split zones to two different instruments simultaneously
- **Pitch bend range mismatch**: External MPE synths often need careful tuning of bending fraction to match Osmose's small physical key travel
- **DIN MIDI bandwidth**: Standard DIN may bottleneck with heavy MPE data from 49 expressive keys — USB strongly preferred
- **Not USB-powered**: Requires external PSU
- **24-voice ceiling**: Voice stealing may occur in dense passages (especially at 12 stereo voices)
- **Keybed feel**: Springy, laterally-mobile action designed for expression — unfamiliar to traditional pianists
- **No front-panel preset editing**: Requires Haken Editor software for deep sound design
