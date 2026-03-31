# Teenage Engineering TX-6

## Overview
- **Type**: Ultra-portable 6-channel stereo mixer + audio interface + synthesizer
- **Channels**: 6 stereo input channels
- **Effects**: 2 FX engines (per-channel sends)
- **Synth**: Built-in per-channel synth (chromatic MIDI input)
- **Sequencer**: Built-in per-channel pattern sequencer
- **Audio Interface**: USB class-compliant, 12-channel, 24-bit, 48 kHz
- **Battery**: Rechargeable, ~8 hours

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | Audio interface (12-ch), MIDI, USB host mode |
| 3.5mm inputs (x6) | In | Stereo line in per channel |
| 3.5mm output (main) | Out | Stereo mix |
| 3.5mm output (aux/cue) | Out | Stereo, independent routing |
| 3.5mm headphone | Out | Stereo, independent level |
| Bluetooth LE | In/Out | MIDI + audio streaming |

### USB Host Mode
The TX-6 can act as a **USB host** — connect class-compliant USB MIDI/audio devices directly without a computer. Can route MIDI between connected devices.

### USB Audio Interface
- 12 channels, 24-bit, 48 kHz (also 44.1 kHz)
- Three routing modes: ext, 12 in, 12 out
- Class-compliant (iOS, macOS, Windows, Linux)

## MIDI Channel Configuration

| Channel | Purpose |
|---------|---------|
| 1-6 | Per-mixer-channel control (volume, pan, EQ, FX sends, synth, sequencer) |
| 7 | Master controls (main volume, transport, tempo, scene recall) |
| 8 | FX I engine parameters |
| 9 | FX II engine parameters |

### MIDI Out
TX-6 sends on **channel 1 only** (all physical controls mapped to CC 1-37). MIDI In/Out modes are **mutually exclusive** — can't send and receive simultaneously.

## MIDI CC Map — Incoming (per channel 1-6)

### Mixer
| CC | Parameter |
|----|-----------|
| 7 | Channel Volume |
| 8 | Pan |
| 9 | Gain |
| 74 | Filter |
| 120 | Mute (toggle) |

### EQ
| CC | Parameter |
|----|-----------|
| 85 | EQ Low |
| 86 | EQ Mid |
| 87 | EQ High |

### Dynamics
| CC | Parameter |
|----|-----------|
| 93 | Compressor |

### FX Sends
| CC | Parameter |
|----|-----------|
| 91 | FX I Send |
| 92 | Aux Send |
| 94 | Cue Send |

### Synth (per channel)
| CC | Parameter |
|----|-----------|
| 3 | Synth Param 1 |
| 89 | Synth Param 2 |
| 90 | Synth Param 3 |
| 95 | Synth Param 4 |

### Sequencer
| CC | Parameter |
|----|-----------|
| 14 | Pattern Select |

## MIDI CC Map — Channel 7 (Master)

| CC | Parameter |
|----|-----------|
| 7 | Main Volume |
| 8 | Aux Volume |
| 9 | Cue Volume |
| 46 | Transport Start/Stop (fw 1.2.15+) |
| 47 | Tempo |
| 122 | Local Control |

### Program Change (Channel 7)
Program Change on channel 7 recalls **scenes**.

## MIDI CC Map — Channels 8-9 (FX Engines)

| CC | Parameter | Ch 8 | Ch 9 |
|----|-----------|------|------|
| 3 | Engine Select | FX I | FX II |
| 89 | Param 1 | FX I | FX II |
| 90 | Param 2 | FX I | FX II |
| 95 | Param 3 | FX I | FX II |
| 91 | Enable/Bypass | FX I | FX II |
| 7 | Return Level | FX I | FX II |

## MIDI CC Map — Outgoing (Channel 1 only)

All physical controls send CCs on channel 1:
| CC Range | Controls |
|----------|----------|
| 1-37 | All faders, knobs, buttons, encoder |

## Sync
- BLE MIDI clock in/out (24 PPQN standard)
- PO-sync output on aux/cue jack (8th or 16th note selectable — for syncing Pocket Operators)
- Transport start/stop via CC 46 on channel 7 (fw 1.2.15+)
- Tempo control via CC 47 on channel 7

## SysEx
None. No SysEx implementation. Firmware updates use `.tfw` files via USB mass storage.

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [darnfish/tx-6-midi-events](https://github.com/darnfish/tx-6-midi-events) | Reverse-engineered BLE MIDI protocol. Note: TX-6 BLE output uses non-standard 16-bit event IDs rather than raw MIDI CC values |

## Limitations
- MIDI Out is channel 1 only
- MIDI In/Out modes are mutually exclusive (can't send and receive simultaneously)
- No per-channel FX II send (only FX I has per-channel send)
- BLE MIDI uses proprietary event encoding (not standard CC over BLE)
- No SysEx, NRPN, or bank select
- Bluetooth audio adds latency
- Small physical size makes knobs cramped
- Limited EQ (no parametric EQ per channel)
- Battery life varies with USB host power draw
- No DIN MIDI
