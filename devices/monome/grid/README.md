# Monome Grid

## Overview
- **Type**: Button matrix controller with variable-brightness LED feedback
- **Sizes**: 64 (8x8), 128 (16x8), 256 (16x16). Current models: "zero" (16x16), "one" (8x16)
- **Buttons**: Silicone, tactile, no velocity. Hardware scans at ~1 kHz.
- **LEDs**: Warm white, 16 brightness levels per button (varibright, 2012+). Older models are monobright (on/off).
- **Design**: Machined aluminum, minimalist

The 128 (16x8) is the most common and best-supported size across the Norns ecosystem.

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB | Bidirectional | serialosc protocol (or direct serial on norns) |

No MIDI. Communicates via OSC over UDP through the serialosc daemon.

## Protocol (mext serial / OSC via serialosc)

### Input (device to host)
| Message | Description |
|---------|-------------|
| `/grid/key x y s` | Button at (x,y) pressed (s=1) or released (s=0) |

### Output (host to device)
| Message | Description |
|---------|-------------|
| `/grid/led/set x y l` | Set LED at (x,y) to level `l` (0-15) |
| `/grid/led/all l` | Set all LEDs to level `l` |
| `/grid/led/map x y d[0..7]` | Set 8x8 quad starting at (x,y) — most efficient bulk update |
| `/grid/led/row x y d` | Set row of 8 LEDs |
| `/grid/led/col x y d` | Set column of 8 LEDs |
| `/grid/led/intensity l` | Set global intensity |

### Tilt (if equipped)
| Message | Description |
|---------|-------------|
| `/tilt n x y z` | Accelerometer data from sensor `n` (8-bit per axis) |

### Performance
- Full 128-grid level map (~130 bytes) takes ~1ms over USB serial
- Full 256 refresh (four 8x8 quads) takes ~2-3ms
- 30-60 fps LED refresh is achievable in practice

## iii Firmware (2022+ grids only)
The new `iii` firmware allows 2022+ grids (one/zero) to run standalone Lua scripts on their RP2040 microcontroller and enumerate as USB-MIDI. Two mutually exclusive modes:
- **Standard serialosc** ("particles") — traditional grid behavior
- **iii scripting** ("plasma") — standalone Lua execution

Toggle via bootloader. Older grids cannot run iii.

## Programming
- **Lua on Norns** — primary use case, extensive `grid` module
- **Max/MSP** — serialosc objects
- **SuperCollider** — [monome/monomeSC](https://github.com/monome/monomeSC)
- **Python** — [monome/libmonome](https://github.com/monome/libmonome) (Python bindings)
- **Rust** — [padenot/monome-rs](https://github.com/padenot/monome-rs)
- **Node.js** — [dinchak/node-monome-grid](https://github.com/dinchak/node-monome-grid)
- **Pure Data** — [boqs/pd-grid](https://github.com/boqs/pd-grid)
- **Processing/Java** — [monome/monome-processing](https://github.com/monome/monome-processing)
- Any language with UDP/OSC support

## Common Patterns
- **Step sequencer**: rows = tracks, columns = steps, brightness = velocity/probability
- **Clip launcher**: grid of clips, brightness shows playback state
- **Instrument**: isomorphic note layout, scales mapped to grid
- **Mixer**: rows as faders, columns as channels
- **Pattern recorder**: record button sequences, play back with variation

## Grid Editions
| Era | LED Type | Protocol | USB |
|-----|----------|----------|-----|
| 2007-2010 | Mono-bright (on/off) | 40h/series | USB-A |
| 2011-2012 | Varibright (16 levels) | mext | USB-A |
| 2015+ | Varibright | mext | USB-C |
| 2022+ | Varibright | mext + iii | USB-C |

Current Norns scripts expect varibright (16 levels). Serial numbers starting with "m" + 7 digits indicate mext protocol.

## 3rd-Party Repos

| Repo | What it does |
|------|-------------|
| [monome/serialosc](https://github.com/monome/serialosc) | Multi-device OSC server daemon (157 stars) |
| [monome/libmonome](https://github.com/monome/libmonome) | C library + Python bindings for all monome hardware (316 stars) |
| [monome/iii](https://github.com/monome/iii) | Embedded Lua scripting firmware for 2022+ grids |
| [okyeron/neotrellis-monome](https://github.com/okyeron/neotrellis-monome) | DIY grid clone using NeoTrellis (234 stars) |
| [miker2049/midigrid](https://github.com/miker2049/midigrid) | Use generic MIDI controllers as grid substitutes on norns |
| [Dewb/monome-rack](https://github.com/Dewb/monome-rack) | VCV Rack plugin emulating monome modules (240 stars) |

## Limitations
- No MIDI output natively (serialosc/OSC only; iii firmware on 2022+ grids can enumerate as USB-MIDI)
- No velocity sensing — buttons are binary (pressed/released)
- No RGB LEDs — warm white only, 16 brightness levels
- Some scripts coded for varibright don't degrade gracefully on older monobright grids
- iii firmware only available on 2022+ models (one/zero)
- Standard and iii modes are mutually exclusive (must toggle via bootloader)
