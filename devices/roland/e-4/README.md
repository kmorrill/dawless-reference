# Roland AIRA Compact E-4

## Overview
- **Type**: Voice transformer with looper
- **Voice effects**: Auto Pitch (pitch correction + robot mode), Harmony (up to 4-voice harmonies), Vocoder
- **Looper**: Overdub, undo/redo, sync to external clock
- **Other effects**: Scatter (beat repeat/glitch), Reverb, low cut filter, noise gate
- **Inputs**: Dynamic mic (XLR-style via 3.5mm), headset mic (via MIX OUT/HEADSET jack)
- **Pitch/Formant**: Continuous pitch slider (+/- 1 octave), formant slider (masculine to feminine)
- **Key/Scale**: Configurable key and scale for pitch correction and harmony generation
- **Firmware**: v1.02

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | Class-compliant USB MIDI + USB audio |
| 3.5mm TRS MIDI IN | In | TRS-to-TRS or TRS-to-DIN cables |
| 3.5mm TRS MIDI OUT | Out | TRS-to-TRS or TRS-to-DIN cables |
| SYNC IN | In | Mono 3.5mm, overrides MIDI clock setting |
| SYNC OUT | Out | Mono 3.5mm |
| MIX IN | In | Stereo 3.5mm audio |
| MIX OUT / HEADSET | Out | Stereo 3.5mm audio + headphones; doubles as headset mic input |
| MIC IN | In | 3.5mm for unbalanced dynamic mic |

## MIDI Implementation

### Channels
- **Default channel**: 3, configurable 1-16
- **Mode**: Mode 3 (Omni Off, Poly)

### Supported Messages (RX)
| Message | Supported | Notes |
|---------|-----------|-------|
| Note On/Off | Yes | Range 0-127; controls vocoder pitch (Robot Note) and harmony voices (up to 4) |
| Pitch Bend | Yes | Controls the pitch slider value |
| Control Change | No | No CC parameters recognized |
| Program Change | No | Not supported |
| SysEx | No | Not supported |
| Clock | Yes | Controls looper timing when syncing |
| Start | Yes | Begins looper sync |
| Continue | Yes | Recognized |
| Stop | Yes | Stops looper |

### Transmitted Messages (TX)
| Message | Supported |
|---------|-----------|
| Clock | Yes |
| Start | Yes |
| Stop | Yes |
| All Sound Off | Yes |
| Active Sensing | Yes |
| Notes / CC / PC | No |

### KEY IN Mode
External MIDI notes drive the vocoder pitch and harmony:
1. Enter menu, select KEY.I
2. Set MIDI channel with SCATTER dial
3. Send Note On/Off + Pitch Bend from external controller

## Sync
| Mode | Description |
|------|-------------|
| AUTO | Accept incoming clocks from any source |
| Int | Internal clock |
| MIDI | Sync to MIDI IN only |
| USB | Sync to USB MIDI only |

- **Looper sync rate**: 1/8, 1/4, 1/2, 1 (relative to beat)
- **MIDI Thru**: Configurable (forwards MIDI IN to MIDI OUT)
- **SYNC IN jack**: Overrides MIDI clock setting when connected

## Physical Controls

### Layout
Unique slider-based design (unlike knob-based S-1/T-8/J-6). Four-digit 7-segment LED display.

### Controls

| Control | Type | Function |
|---------|------|----------|
| VOLUME | Knob | MIX OUT volume |
| SENS | Knob | Microphone input gain |
| SCATTER | Knob | Scatter effect (chops audio into rhythmic fragments) |
| PITCH | Slider | Voice pitch shift (+/- 1 octave, semitone increments) |
| FORMANT | Slider | Voice formant shift (masculine <-> feminine character) |
| REVERB | Slider | Reverb level; reassignable to Echo, Delay, or Chorus via VOCODER button |
| AUTO PITCH | Button | Enable pitch correction / hard-tune effect |
| HARMONY | Button | Add vocal harmonies; hold + PITCH slider to set key |
| VOCODER | Button | Enable vocoder; hold + REVERB slider to change effect type |
| LOOPER | Button | Looper record/play/overdub |

Total: 3 knobs, 3 sliders, 4 effect buttons.

### Notable

| Repo | Description |
|------|-------------|
| [sonyhome/roland-e4-quick-reference](https://github.com/sonyhome/roland-e4-quick-reference) | E-4 quick reference card |

## Limitations
- No CC control of any parameters (pitch, formant, scatter, reverb, etc. are not MIDI-controllable)
- No Program Change support
- No SysEx
- No Song Position Pointer / Song Select
- Does not transmit notes, CC, or Program Change

## Detailed Docs
- [MIDI Implementation (external control reference)](midi-implementation.md)
- [Owner's Manual](pdfs/E-4-manual.md)
