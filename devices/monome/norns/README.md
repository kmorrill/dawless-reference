# Monome Norns

## Overview
- **Type**: Sound computer — Lua scripting + SuperCollider audio engine
- **Models**: Norns (original), Norns Shield (DIY, Raspberry Pi-based — no longer officially supported)
- **CPU**: Raspberry Pi CM3/CM3+ — quad-core ARM Cortex-A53 @ 1.2 GHz, 1 GB RAM
- **Storage**: 4 GB (CM3 pre-2021) or 32 GB (CM3+ 2021+)
- **Audio**: Stereo in/out, 1/4" jacks, CS4270 codec, fixed 48 kHz, 24-bit
- **Screen**: 128x64 OLED, 16 brightness levels, monochrome
- **Controls**: 3 encoders, 3 buttons
- **OS**: Linux-based (real-time kernel), custom Norns software stack

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB Host (x4) | In | Connects Grid, Arc, MIDI controllers, other USB devices |
| WiFi | Bidirectional | SSH access, script management via Maiden web IDE |
| Audio In | In | Stereo 1/4", line-level only (no mic preamp), 10k ohm |
| Audio Out | Out | Stereo 1/4", balanced or unbalanced, 590 ohm |
| Headphone | Out | 1/4" |

## MIDI
- Acts as a **USB MIDI host** — connects class-compliant USB MIDI devices
- Can send and receive MIDI (note, CC, clock, program change, sysex)
- MIDI routing configured per-script in Lua
- Supports up to 16 simultaneous MIDI devices
- No DIN MIDI (unless via USB adapter)

## Scripting (Lua)
Scripts define everything: audio processing, UI, MIDI behavior, Grid/Arc interaction.

```lua
-- Example: receive MIDI note, play sound
m = midi.connect()
m.event = function(data)
  local msg = midi.to_msg(data)
  if msg.type == "note_on" then
    engine.hz(midi_to_hz(msg.note))
  end
end
```

### Key APIs
| Module | Purpose |
|--------|---------|
| `engine` | SuperCollider audio engine control |
| `midi` | MIDI I/O |
| `grid` | Grid button/LED control |
| `arc` | Arc encoder/LED control |
| `osc` | OSC send/receive |
| `clock` | Tempo-synced coroutines, MIDI clock |
| `params` | User-facing parameter system |
| `screen` | OLED drawing |
| `softcut` | 6-voice stereo sample player/looper |

## Audio Engine (SuperCollider)
- Custom SuperCollider engines loaded per-script
- **Softcut**: built-in 6-voice buffer manipulation engine — 2 mono buffers of ~5 min 50 sec each (16,777,216 frames at 48 kHz), max block size 2048 frames
- Full SuperCollider synthesis available
- **Audio codec**: CS4270 I2S, externally clocked (crystal, no jitter), fixed 48 kHz
- Input impedance: 10k ohm (balanced or unbalanced)
- Output impedance: 590 ohm (balanced or unbalanced)
- Typical round-trip latency: ~10-15ms (128 or 256 frame JACK buffer at 48 kHz)

## OSC
- Full OSC send/receive over network (WiFi)
- OSC is also the internal communication layer between Lua and SuperCollider
- Can communicate with other OSC-capable devices/software on the network

## Script Ecosystem
- **400+ community scripts** indexed at https://norns.community
- Scripts installed via Maiden web IDE or git
- Categories: sequencers, loopers, synths, effects, generative, utilities
- Curated index: [p3r7/awesome-monome-norns](https://github.com/p3r7/awesome-monome-norns)

## 3rd-Party Repos

### Official
| Repo | What it does |
|------|-------------|
| [monome/norns](https://github.com/monome/norns) | Main norns platform (719 stars) |
| [monome/dust](https://github.com/monome/dust) | Default script collection |
| [monome/maiden](https://github.com/monome/maiden) | Web-based IDE/REPL |
| [monome/softcut-lib](https://github.com/monome/softcut-lib) | Core sample-cutting DSP library (C++) |
| [monome/crow](https://github.com/monome/crow) | Scriptable USB-CV-II bridge to modular |

### Notable Community Scripts
| Repo | What it does |
|------|-------------|
| [itsyourbedtime/orca](https://github.com/itsyourbedtime/orca) | Lua port of ORCA (100 stars) |
| [schollz/oooooo](https://github.com/schollz/oooooo) | 6x digital tape loops |
| [jaseknighter/flora](https://github.com/jaseknighter/flora) | L-systems sequencer |
| [andr-ew/ndls](https://github.com/andr-ew/ndls) | 4-track tape looper for norns+grid |
| [dstroud/dreamsequence](https://github.com/dstroud/dreamsequence) | Chord-based sequencer/arpeggiator/harmonizer |
| [tehn/mlr](https://github.com/tehn/mlr) | Classic live sample-cutting instrument |
| [tehn/awake](https://github.com/tehn/awake) | Canonical grid step sequencer |

### DIY / Extensions
| Repo | What it does |
|------|-------------|
| [monome/norns-shield](https://github.com/monome/norns-shield) | DIY shield PCB + BOM (602 stars) |
| [okyeron/shieldXL](https://github.com/okyeron/shieldXL) | Remixed norns shield with extras |
| [miker2049/midigrid](https://github.com/miker2049/midigrid) | Use Launchpad etc. as grid substitutes |
| [Dewb/monome-rack](https://github.com/Dewb/monome-rack) | VCV Rack plugin emulating monome eurorack modules |

## Limitations
- **CPU ceiling**: Pi CM3 ARM @ 1.2 GHz shared between Lua and SuperCollider. Complex synth engines top out at 4-8 voices before xruns.
- **Fixed 48 kHz**: All imported audio must be 48 kHz or plays back at wrong speed/pitch
- **Single engine**: Only one SuperCollider engine loaded at a time per script. Switching causes audio gap.
- **Lua single-threaded**: Heavy computation in callbacks blocks the event loop
- **Max 36 metros**: Hardcoded concurrent timer limit
- **4 GB storage** on pre-2021 units is very tight for sample-heavy work
- **No Bluetooth**: Not supported even with USB adapter
- **WiFi**: Requires specific Ralink RT5370 chipset for hotspot mode; can be flaky
- **Pi 4 issues**: Physical footprint mismatch with shield, USB/ethernet port differs, can short against PCB
- **Ground loop noise**: Common when USB charging and audio input share same source
- **Line-level only**: No mic preamp or instrument-level input
- **Screen**: Very small (128x64), monochrome, practical refresh 15-30 fps
