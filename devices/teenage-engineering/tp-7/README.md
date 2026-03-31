# Teenage Engineering TP-7

## Overview
- **Type**: Portable field recorder / tape machine / USB audio interface
- **Recording**: 24-bit, up to 96 kHz, multitrack (up to 6 stereo pairs)
- **Storage**: 128 GB internal (no SD card)
- **Display**: 64x32 pixel monochrome OLED
- **Battery**: Li-Ion, ~7 hours, USB-C 5V charging
- **Dimensions**: 96 x 68 x 16 mm, 170g
- **Modes**: Memo (quick single-track), Record (full multitrack), Library (playback)

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | Audio interface (6-ch), MIDI, file transfer, charging |
| 3.5mm TRRS (x3) | In/Out | Each independently configurable as stereo input, stereo output, or headset |
| Bluetooth LE | Out | MIDI control + iOS companion app (no BT audio streaming) |
| Built-in mic | In | Mono condenser, SNR 98 dBA |
| Built-in speaker | Out | Mono monitoring |

### 3.5mm TRRS Jack Specs
| Mode | Spec |
|------|------|
| Input | 9.5 kOhm impedance, 0-42 dB analog gain, max 8 dBu, SNR 105 dBA |
| Headphone out | Max 8 dBu (2 Vrms), SNR 120 dBA |
| Headset out | Max 2 dBu (1 Vrms), SNR 110 dBA |

No XLR, no phantom power. 6.35mm adapter included.

## USB Audio Interface
- 6-channel (3 stereo pairs), 24-bit, up to 96 kHz
- Class-compliant (iOS/macOS/Windows/Linux)
- Tracks 1-3 available over USB; internal mixer handles all 6 tracks

## MIDI Implementation

Controller Mode (sending) and receiving are **mutually exclusive** — cannot do both simultaneously. The TP-7 never reports its internal state via MIDI.

### Incoming MIDI (control the TP-7)

| Message | Channel | Function |
|---------|---------|----------|
| Note On/Off 0-127 | Any | Trigger/recall cue markers (requires MIDI-CUE enabled) |
| Pitch Bend ±8192 | Any | Playback speed multiplier (0.25x to 2.0x, asymmetric curve) |
| CC 7 (0-127) | 1-6 | Mix volume per stereo track |
| CC 9 (0-127) | 1-3 | Input gain per channel (0-42 dB) |
| CC 14 (on/off) | Any | Arm recording |
| CC 16 (on/off) | Any | Cue rec mode enable |
| CC 17 (0-2) | Any | Loop control (0=exit, 1=set IN, 2=set OUT) |
| CC 18 (0-127) | Any | Transport/rocker emulation (behavior differs when stopped vs playing) |
| CC 120 (on/off) | 1-6 | Mute per channel |

### Outgoing MIDI (Controller Mode, channel 1 only)

| Control | Message | Details |
|---------|---------|---------|
| Up/Down/Rec/Play/Stop/Left/Right/Memo buttons | CC 20-27 | 0-127 button press |
| Wheel (motorized reel) | CC 30 | Relative two's complement, -64 to +63 |
| Rocker switch | Pitch Bend | ±8192, variable-speed forward/reverse |

### Gotchas
- CC 18 transport control has two completely different behaviors depending on whether playback is active
- Stopping during playback requires CC 18=60 combined with Pitch Bend=+708
- Three independent speed controls multiply during playback: on-screen SPD (no MIDI access), Rocker/CC 18, and Pitch Bend
- Pitch Bend speed is invisible on the device display
- Mixer levels and mutes reset when track changes via cue or loop trigger
- Recording via MIDI (CC 14) is limited — can arm but can't fully control transport in record mode

## Physical Controls
| Control | Function |
|---------|----------|
| Motorized tape reel (center disc) | Rotary encoder for scrubbing/scrolling (CC 30 in controller mode) |
| Rocker switch | Variable-speed forward/reverse playback (Pitch Bend in controller mode) |
| REC button (red) | Start/arm recording |
| PLAY button | Start playback |
| STOP button | Stop transport |
| MEMO button | Instant recording — powers on device and starts recording even from off state |
| UP / DOWN buttons | Navigation, volume adjustment |
| LEFT / RIGHT buttons | Navigation, menu |
| Mode button | Switch between modes |

## Recording Formats
- **WAV**: 24-bit, up to 96 kHz (primary format)
- **Multitrack**: Custom interleaved WAV, 2-12 channels (requires tp7-util or TP-7 app to split)
- **FLAC**: Playback supported
- **MP3**: Playback supported (fw 1.1.9+)
- **Bounce**: Internal merge of multiple tracks to single stereo file

## Firmware History
| Version | Key Features |
|---------|-------------|
| 1.1.3 | Cue points, loop IN/OUT, MIDI controller mode, multi-language transcription, bounce/mixdown |
| 1.1.9 | MP3 playback, mix-down mode (live routing to TX-6), extended MIDI, extended gain range |
| 1.1.11 | Latest known firmware, basis for community MIDI spec |

## SysEx
None. Firmware updates use `.tfw` files via USB mass storage.

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [lucidyan/tp7-midi](https://github.com/lucidyan/tp7-midi) | Web app for controlling TP-7 via Web MIDI/BLE. Includes the most comprehensive community MIDI spec (`MIDI_SPEC.md`) |
| [mellson/tp7-util](https://github.com/mellson/tp7-util) | Native macOS Swift app for converting TP-7 multitrack WAV to/from individual stereo WAV files |

## Limitations
- No SD card slot — 128 GB internal only
- No XLR / phantom power — 3.5mm TRRS only
- Controller Mode vs Receive Mode mutually exclusive
- No MIDI state feedback — device never reports current state
- No MIDI access to on-screen SPD (speed display)
- Mixer resets on track change (cue/loop triggers)
- Motorized reel mechanism sensitive to dust/humidity
- No trimming/cutting of individual tracks — recordings expand to longest track length
- One loop per recording — cannot store/browse multiple loops
- Recording via MIDI is limited (arm only, not full transport control)
- No DIN MIDI
- No Bluetooth audio streaming (BLE is MIDI/app only)
