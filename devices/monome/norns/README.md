# Monome Norns

## Overview
- **Type**: Sound computer — Lua scripting + SuperCollider audio engine
- **Models**: Norns (original), Norns Shield (DIY, Raspberry Pi-based)
- **Audio**: Stereo in/out, 1/4" jacks
- **Screen**: 128x64 OLED
- **Controls**: 3 encoders, 3 buttons
- **OS**: Linux-based, custom Norns software stack

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB Host | In | Connects Grid, Arc, MIDI controllers, other USB devices |
| WiFi | Bidirectional | SSH access, script management via Maiden web IDE |
| Audio In | In | Stereo 1/4" |
| Audio Out | Out | Stereo 1/4" |
| Headphone | Out | 1/4" |

## MIDI
- Acts as a **USB MIDI host** — can connect class-compliant USB MIDI devices
- Can send and receive MIDI (note, CC, clock, program change)
- MIDI routing configured per-script in Lua
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
- **Softcut**: built-in 6-voice buffer manipulation engine (looping, delay, granular)
- Full SuperCollider synthesis available
- **Audio codec**: CS4270 I2S, externally clocked (crystal, no jitter), fixed 48 kHz
- Input impedance: 10k ohm (balanced or unbalanced)
- Output impedance: 590 ohm (balanced or unbalanced)

## OSC
- Full OSC send/receive over network (WiFi)
- OSC is also the internal communication layer between Lua and SuperCollider
- Can communicate with other OSC-capable devices/software on the network

## Script Ecosystem
- Hundreds of community scripts at https://norns.community
- Scripts installed via Maiden web IDE or git
- Categories: sequencers, loopers, synths, effects, generative, utilities
