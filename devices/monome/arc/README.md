# Monome Arc

## Overview
- **Type**: High-resolution optical encoder controller with LED ring feedback
- **Models**: Arc 2 (2 encoders), Arc 4 (4 encoders)
- **Encoders**: Continuous rotation, optical, no detents, ~256 ticks/revolution
- **LEDs**: 64 per ring, 16 brightness levels (4-bit), warm white
- **Design**: Machined aluminum, no buttons, no screen — pure encoder control
- **Availability**: Small batch production, check monome.org

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB | Bidirectional | serialosc protocol (same as Grid) |

No MIDI. No DIN. No Bluetooth. Communicates via OSC over UDP through the serialosc daemon.

## Protocol (OSC via serialosc)

### Input (device to host)
| Message | Description |
|---------|-------------|
| `/enc/delta n d` | Encoder `n` turned by delta `d` (signed integer, speed-sensitive) |

### Output (host to device)
| Message | Description |
|---------|-------------|
| `/ring/set n x l` | Set LED `x` on ring `n` to level `l` (0-15) |
| `/ring/all n l` | Set all 64 LEDs on ring `n` to level `l` |
| `/ring/map n d[0..63]` | Set all 64 LEDs on ring `n` in one message |
| `/ring/range n x1 x2 l` | Set a range of LEDs on ring `n` |

### System
| Message | Description |
|---------|-------------|
| `/sys/info` | Request device info |
| `/sys/id` | Device serial |
| `/sys/size` | Number of encoders |
| `/sys/prefix` | OSC prefix |

## Programming
- **Lua on Norns** — first-class support, many scripts support Arc natively
- **Max/MSP** — official serialosc objects
- **SuperCollider** — community serialosc libraries
- **Python** — `pymonome` library
- **Any language** with UDP/OSC can control it

## Musical Uses
- Continuous parameter control (filter, mix, effects)
- Sequencer step editing and pattern manipulation
- Modulation source (LFO rate/depth, envelope shape)
- Granular synthesis control
- Excellent for cyclical/wrapping parameters due to ring visualization
