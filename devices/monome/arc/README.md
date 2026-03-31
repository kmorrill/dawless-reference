# Monome Arc

## Overview
- **Type**: High-resolution rotary encoder controller with LED ring feedback
- **Models**: Arc 2 (2 encoders), Arc 4 (4 encoders). 2025 edition adds a pushbutton.
- **Encoders**: Optical, continuous rotation, 1024 ticks per revolution, no detents
- **LEDs**: 64 per encoder ring, 16 brightness levels (0-15), warm white. LED 0 = north, numbered clockwise.
- **Design**: Machined aluminum, minimalist

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB | Bidirectional | serialosc protocol (same as Grid) |

No MIDI natively. Communicates via OSC over UDP through the serialosc daemon.

## Protocol (mext serial / OSC via serialosc)

### Input (device to host)
| Message | Description |
|---------|-------------|
| `/enc/delta n d` | Encoder `n` rotated by delta `d` (signed integer, positive = clockwise) |
| `/enc/key n s` | Encoder `n` button pressed (s=1) / released (s=0) — **2025 model only** |

### Output (host to device)
| Message | Description |
|---------|-------------|
| `/ring/set n x l` | Set LED `x` on ring `n` to level `l` (0-15) |
| `/ring/all n l` | Set all LEDs on ring `n` to level `l` |
| `/ring/map n d[0..63]` | Set all 64 LEDs on ring `n` in one message |
| `/ring/range n x1 x2 l` | Set clockwise arc from `x1` to `x2` to level `l` (wraps) |

### System
| Message | Description |
|---------|-------------|
| `/sys/info` | Request device info |
| `/sys/id` | Device serial |
| `/sys/size` | Number of encoders |
| `/sys/prefix` | OSC prefix |

### Performance
- A full ring map is 64 bytes + header per encoder
- Refreshing all 4 rings at 60 fps is feasible but approaches USB serial bandwidth limits

## Programming
- **Lua on Norns** — first-class `arc` module support
- **Max/MSP** — serialosc objects
- **SuperCollider** — [monome/monomeSC](https://github.com/monome/monomeSC)
- **Python** — [monome/libmonome](https://github.com/monome/libmonome) (Python bindings)
- **Rust** — [padenot/monome-rs](https://github.com/padenot/monome-rs)
- Any language with UDP/OSC support

## Common Uses
- Continuous parameter control (filter cutoff, mix levels, effect params)
- Sequencer step editing and pattern manipulation
- Modulation source (LFO rate/depth, envelope shape)
- Granular synthesis control (position, grain size, density)
- Mixing / crossfading
- LED rings excel at representing cyclical/wrapping values

## iii Firmware (2025 Arc only)
The 2025 Arc (with pushbutton) supports the `iii` firmware for standalone Lua scripting on RP2040, same as 2022+ grids. Older arcs are serialosc-only.

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [monome/serialosc](https://github.com/monome/serialosc) | Multi-device OSC server daemon |
| [monome/libmonome](https://github.com/monome/libmonome) | C library + Python bindings (316 stars) |
| [monome/iii](https://github.com/monome/iii) | Embedded Lua scripting firmware (2025 arc + 2022+ grids) |
| [TheSlowGrowth/MonomeArcClone](https://github.com/TheSlowGrowth/MonomeArcClone) | Full arc clone, works with serialosc (140 stars) |

## Limitations
- **Delta-only output**: Reports relative rotation, not absolute position. Software must maintain state.
- **No detents/haptics**: Pure optical encoders with no physical feedback
- **No built-in acceleration curves**: Software must implement velocity/acceleration scaling on delta values
- **Button only on 2025 model**: Older arcs have no button input (`/enc/key` only works on 2025+)
- **iii firmware only on 2025 model**: All older arcs are serialosc-only
- **No MIDI natively**: OSC via serialosc (or iii USB-MIDI on 2025 model)
- **Availability**: Produced in small batches, often sold out, $400-600 range
