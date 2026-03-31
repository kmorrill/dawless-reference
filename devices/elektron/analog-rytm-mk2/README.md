# Elektron Analog Rytm MKII

## Overview
- **Type**: Analog drum machine / sampler
- **Voices**: 12 analog voices (8 main tracks + 4 shared via voice pairing)
- **Tracks**: 13 (12 drum + 1 FX)
- **Machines**: 34 drum synthesis engines across BD, SD, RS, CP, BT, CY, CH, OH, HH, CB, SY, UT categories
- **Pads**: 12 velocity/pressure-sensitive rubber pads
- **Sequencer**: 64 steps per pattern, parameter locks, micro-timing, conditional trigs, Euclidean mode
- **Sampling**: Mono sampling via audio input, +Drive storage (1GB)
- **Effects**: Analog distortion + digital delay/reverb/compressor (send FX on dedicated FX track)
- **Display**: OLED screen
- **Patterns**: 8 banks × 16 patterns = 128 patterns

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B | In/Out | MIDI + Overbridge (multi-channel audio/control) or class-compliant USB audio/MIDI |
| MIDI DIN Out | Out | 5-pin DIN (configurable: MIDI / DIN24 / DIN48) |
| MIDI DIN In | In | 5-pin DIN |
| MIDI DIN Thru | Thru | 5-pin DIN |
| Audio Out (main) | Out | 2x 1/4" TRS (L/R balanced) |
| Audio Out (individual) | Out | 8x 1/4" (tracks 1-8, unbalanced) |
| Headphone | Out | 1/4" stereo |
| Audio In | In | 2x 1/4" (L/R, for sampling + external processing) |
| Expression / CV | In | 1/4" (configurable) |

### USB Modes (mutually exclusive)
| Mode | Capability |
|------|-----------|
| Overbridge | Proprietary VST/AU plugin: multi-channel audio, full parameter control, sample transfer, total recall |
| USB MIDI | MIDI only |
| USB Audio/MIDI | Class-compliant multi-channel audio + MIDI (no drivers needed) |

## MIDI Channel Configuration

| Channel | Purpose |
|---------|---------|
| Track 1-12 | Individual per-track channels (assignable, or OFF) |
| FX Track | Separate channel for delay/reverb/distortion/compressor CCs |
| Auto Channel | Controls whichever track is currently active (useful for keyboard input) |
| Perf Channel | Performance macro parameters (CC 35-47) |
| Program CH In | Listens for pattern changes |
| Program CH Out | Sends pattern changes |

All channels configurable in GLOBAL > MIDI CONFIG.

## Voice Sharing

Tracks sharing voice circuits (right track has priority, mutes left if simultaneous):
- RS (3) / CP (4)
- MT (7) / HT (8)
- CH (9) / OH (10)
- CY (11) / CB (12)

## MIDI CC Map

**Prerequisite**: GLOBAL > MIDI CONFIG > RECEIVE CC/NRPN = ON

All CCs also have NRPN equivalents for higher resolution where applicable.

### Track Trigger Parameters (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 3 | Note | 3:0 |
| 4 | Velocity | 3:1 |
| 5 | Length | 3:2 |
| 11 | Synth Trig | 3:3 |
| 12 | Sample Trig | 3:4 |
| 13 | ENV Trig | 3:5 |
| 14 | LFO Trig | 3:6 |

### Euclidean Sequencer (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 86 | Pulse Generator 1 | 3:8 |
| 87 | Pulse Generator 2 | 3:9 |
| 88 | Boolean Operator | 3:10 |
| 89 | Rotation Generator 1 | 3:11 |
| 90 | Rotation Generator 2 | 3:12 |
| 91 | Track Rotation | 3:13 |
| 117 | Euclidean on/off | 3:14 |

### Track Control (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 15 | Machine Type | 1:103 |
| 92 | Active Scene | 1:104 |
| 93 | Track Solo | 1:102 |
| 94 | Track Mute | 1:101 |
| 95 | Track Level | 1:100 |

### Synth Parameters (send on track channel)
CCs 16-23 map to Synth Param 1-8 — meaning varies per machine type. See [Machine-Specific Parameters](#machine-specific-parameters) below.

| CC | Parameter | NRPN |
|----|-----------|------|
| 16-23 | Synth Param 1-8 | 1:0 to 1:7 |

### Sample Control (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 24 | Sample Tune | 1:8 |
| 25 | Sample Fine Tune | 1:9 |
| 26 | Bit Reduction | 1:10 |
| 27 | Sample Slot | 1:11 |
| 28 | Start | 1:12 |
| 29 | End | 1:13 |
| 30 | Loop | 1:14 |
| 31 | Level | 1:15 |

### Filter (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 70 | Attack | 1:16 |
| 71 | Decay | 1:17 |
| 72 | Sustain | 1:18 |
| 73 | Release | 1:19 |
| 74 | Frequency | 1:20 |
| 75 | Resonance | 1:21 |
| 76 | Mode | 1:22 |
| 77 | Env Depth | 1:23 |

### Amp (send on track channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 78 | Attack | 1:24 |
| 79 | Hold | 1:25 |
| 80 | Decay | 1:26 |
| 81 | Overdrive | 1:27 |
| 82 | Delay Send | 1:28 |
| 83 | Reverb Send | 1:29 |
| 10 | Pan | 1:30 |
| 7 | Volume | 1:31 |

### LFO (send on track channel)
| CC | Parameter | NRPN | Resolution |
|----|-----------|------|------------|
| 102 | Speed | 1:32 | 7-bit |
| 103 | Multiplier | 1:33 | 7-bit |
| 104 | Fade In/Out | 1:34 | 7-bit |
| 105 | Destination | 1:35 | 7-bit |
| 106 | Waveform | 1:36 | 7-bit |
| 107 | Start Phase | 1:37 | 7-bit |
| 108 | Trig Mode | 1:38 | 7-bit |
| 109 (MSB) + 118 (LSB) | Depth | 1:39 | 14-bit |

### Performance Macros (send on Perf channel)
| CC | Parameter |
|----|-----------|
| 35-46 | Performance 1-12 |

### FX Track — Delay (send on FX channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Time | 2:0 |
| 17 | Pingpong | 2:1 |
| 18 | Stereo Width | 2:2 |
| 19 | Feedback | 2:3 |
| 20 | HPF | 2:4 |
| 21 | LPF | 2:5 |
| 22 | Reverb Send | 2:6 |
| 23 | Mix Volume | 2:7 |

### FX Track — Reverb (send on FX channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 24 | Predelay | 2:8 |
| 25 | Decay Time | 2:9 |
| 26 | Shelving Freq | 2:10 |
| 27 | Shelving Gain | 2:11 |
| 28 | HPF | 2:12 |
| 29 | LPF | 2:13 |
| 31 | Mix Volume | 2:15 |

### FX Track — Distortion (send on FX channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 70 | Amount | 2:16 |
| 71 | Symmetry | 2:17 |
| 72 | Delay Overdrive | 2:18 |
| 76 | Delay Dist/Comp Routing | 2:22 |
| 77 | Reverb Dist/Comp Routing | 2:23 |

### FX Track — Compressor (send on FX channel)
| CC | Parameter | NRPN |
|----|-----------|------|
| 78 | Threshold | 2:24 |
| 79 | Attack | 2:25 |
| 80 | Release | 2:26 |
| 81 | Makeup | 2:27 |
| 82 | Ratio | 2:28 |
| 83 | Sidechain EQ | 2:29 |
| 84 | Dry/Wet | 2:30 |
| 85 | Output | 2:31 |

### NRPN Format
```
CC 99 (NRPN MSB): High byte
CC 98 (NRPN LSB): Low byte
CC 6  (Data Entry MSB): Value MSB
CC 38 (Data Entry LSB): Value LSB (for 14-bit)
```

## Machine-Specific Parameters

CCs 16-23 (Synth Param 1-8) have different meanings per machine type. All 34 machines listed below.

### Bass Drum (7 machines)
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| BD ACOUSTIC | Level | Tune | Decay | Sweep Depth | Sweep Time | Hold Time | Impact | Waveform |
| BD CLASSIC | Level | Tune | Decay | Hold | Sweep Time | Sweep Depth | Waveform | Transient Tick |
| BD FM | Level | Tune | Decay | FM Amount | Sweep Time | FM Sweep Time | FM Decay Time | FM Tune |
| BD HARD | Level | Tune | Decay | Hold | Sweep Time | Sweep Depth | Waveform | Transient Tick |
| BD PLASTIC | Level | Tune | Decay Time | Sweep Depth | Sweep Time | Hold Time | VCO Click | Dust Level |
| BD SHARP | Level | Tune | Decay | Sweep Depth | Sweep Time | Hold Time | Tick Level | Waveform |
| BD SILKY | Level | Tune | Decay | Sweep Depth | Sweep Time | Hold | VCO Click | Dust Level |

### Snare Drum (5 machines)
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| SD ACOUSTIC | Level | Tune | Decay | Noise Decay | Hold Time | Noise Level | Impact | Sweep Depth |
| SD CLASSIC | Level | Tune | Decay | Detune | Snap Amount | Noise Decay | Noise Level | Osc Balance |
| SD FM | Level | Tune | Decay | FM Tune | FM Decay Time | Noise Decay | Noise Level | FM Amount |
| SD HARD | Level | Tune | Decay | Sweep Depth | Tick Level | Noise Decay | Noise Level | Sweep Time |
| SD NATURAL | Level | Tune | Body Decay | Noise Decay | Noise LPF | Noise Balance | Noise Resonance | Noise HPF |

### Rimshot / Clap / Tom
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| RS CLASSIC | Level | Tune Osc 1 | Decay | Osc Balance | Tune Osc 2 | Symmetry | Noise Level | Tick Level |
| RS HARD | Level | Tune | Decay | Sweep Depth | Tick Level | Noise Level | Symmetry | Sweep Time |
| CP CLASSIC | Level | Noise Tone | Noise Decay | Clap Number | Clap Rate | Noise Level | Random Claps | Clap Decay |
| BT CLASSIC | Level | Tune | Decay | Sweep Depth | Sweep Time | Snap Type | Noise Level | Noise Tone |

### Cymbals (3 machines)
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| CY CLASSIC | Level | Tune | Decay | Color | Tone | — | — | — |
| CY METALLIC | Level | Tune | Decay Time | Tone | Transient Decay | — | — | — |
| CY RIDE | Level | Tune | Tail Decay | Hit Decay | Cymbal Type | Component 1 | Component 2 | Component 3 |

### Hi-Hats (6 machines)
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| CH CLASSIC | Level | Tune | Decay | Color | — | — | — | — |
| CH METALLIC | Level | Tune | Decay Time | — | — | — | — | — |
| OH CLASSIC | Level | Tune | Decay | Color | — | — | — | — |
| OH METALLIC | Level | Tune | Decay Time | — | — | — | — | — |
| HH BASIC | Level | Tune | Decay Time | Tone | Transient Decay | Osc Reset | — | — |
| HH LAB | Level | Tune 1 | Decay Time | Tune 2 | Tune 3 | Tune 4 | Tune 5 | Tune 6 |

### Cowbell / Synth / Utility
| Machine | P1 | P2 | P3 | P4 | P5 | P6 | P7 | P8 |
|---------|----|----|----|----|----|----|----|----|
| CB CLASSIC | Level | Tune | Decay Time | Detune | — | — | — | — |
| CB METALLIC | Level | Tune | Decay Time | Detune | — | — | — | — |
| SY CHIP | Level | Tune | Decay | Waveform | Speed | Offset 2 | Offset 3 | Offset 4 |
| SY DUAL VCO | Level | Osc 1 Tune | Osc 1 Decay | Balance | Osc 2 Detune | Osc Config | Osc 2 Decay | Bend |
| SY RAW | Level | Tune | Decay | Noise Level | Osc 2 Detune | Waveform 1 | Waveform 2 | Balance |
| UT IMPULSE | Level | Attack | Decay | Polarity | — | — | — | — |
| UT NOISE | Level | LP Frequency | Decay | Sweep Depth | Sweep Time | LP Resonance | HP Frequency | Attack |

## Program Change

Pattern selection via Program Change:
```
PC 0-15    = Bank A (A01-A16)
PC 16-31   = Bank B (B01-B16)
PC 32-47   = Bank C (C01-C16)
PC 48-63   = Bank D (D01-D16)
PC 64-79   = Bank E (E01-E16)
PC 80-95   = Bank F (F01-F16)
PC 96-111  = Bank G (G01-G16)
PC 112-127 = Bank H (H01-H16)
```

Send on the Program Change In channel.

## MIDI Note Mapping

| Note Range | Function |
|-----------|----------|
| 0-11 (C-1 to B-1) | Trigger tracks 1-12 |
| 12-59 (C0 to B3) | Chromatic playback on active track (requires chromatic mode) |

## SysEx Protocol

Reverse-engineered community protocol. Manufacturer ID: `0x00 0x20 0x3C`, Product ID: `0x07`.

### Request Format (15 bytes)
```
F0 00 20 3C 07 00 <ID> 01 01 <NR> 00 00 00 05 F7
```

### Object Types
| Type | Dump ID (saved/work) | Request ID (saved/work) | Raw Size |
|------|---------------------|------------------------|----------|
| Kit | 0x52 / 0x58 | 0x62 / 0x68 | 2,610 bytes |
| Sound | 0x53 / 0x59 | 0x63 / 0x69 | 162 bytes |
| Pattern | 0x54 / 0x5A | 0x64 / 0x6A | 13,101 bytes |
| Song | 0x55 / 0x5B | 0x65 / 0x6B | — |
| Settings | 0x56 / 0x5C | 0x66 / 0x6C | — |
| Global | 0x57 / 0x5D | 0x67 / 0x6D | 80 bytes |

### Pattern Structure
- 64 steps × 13 tracks (12 drum + 1 FX)
- 72 parameter lock sequences (6 per track)
- Per-step: note, velocity, length, trig conditions, micro-timing

### Kit Structure
- 12 drum sounds (162 bytes each) + FX track settings
- Per-track: machine type, 8 synth params, sample control, filter, amp, LFO
- Track levels, retrig settings, FX LFO destinations

## Sync
- MIDI clock send/receive (USB and DIN)
- DIN MIDI Out configurable as: MIDI / DIN24 / DIN48
- Transport start/stop send/receive
- Program change send/receive

## Software
- **Elektron Transfer** — sample management, project backup
- **Overbridge** — multi-channel USB audio, DAW integration, full parameter control, total recall
- **Elektron OS** — firmware updates via USB

## 3rd-Party Repos & Libraries

| Repo | Language | What it does |
|------|----------|-------------|
| [kmorrill/rytm](https://github.com/kmorrill/rytm) | Rust + Web | Comprehensive control suite: SysEx communication, web-based sequencer editor, pattern generation, song arrangement, genre-based pattern tools |
| [bsp2/libanalogrytm](https://github.com/bsp2/libanalogrytm) | C | Low-level SysEx encoding/decoding, reverse-engineered data structures for kit/sound/pattern/global objects |
| [kmorrill/rytm-transcribe](https://github.com/kmorrill/rytm-transcribe) | Python | Audio → rytmloop-2.0 JSON: onset detection, hit classification, quantization, velocity extraction for pattern import |

## Formats

### rytmloop-2.0 (JSON)
Pattern import/export format supporting:
- Per-track: machine type, step events (velocity, length, note, conditions, parameter locks)
- Scenes and performance macros (optional)
- Song arrangement (optional)

See [rytm repo docs](https://github.com/kmorrill/rytm/blob/main/docs/rytmloop-2.0.md) for full specification.

## Limitations
- Mono sampling only (no stereo sample recording)
- 12-voice limit (voices shared across tracks via pairing)
- No time-stretching for samples
- Overbridge requires dedicated Elektron software (not class-compliant in Overbridge mode)
- Individual outputs are unbalanced 1/4" (main outs are balanced)
- No CV output (has CV/expression input only)
- SysEx protocol is reverse-engineered, not officially documented — firmware updates may break compatibility
- All CC/NRPN maps based on OS 1.72
