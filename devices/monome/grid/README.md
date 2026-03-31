# Monome Grid

## Overview
- **Type**: Button matrix controller with variable-brightness LED feedback
- **Sizes**: 64 (8x8), 128 (16x8), 256 (16x16)
- **Buttons**: Silicone, tactile, no velocity
- **LEDs**: Warm white, 16 brightness levels per button (varibright)
- **Design**: Machined aluminum, minimalist

The 128 (16x8) is the most common and best-supported size across the Norns ecosystem.

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB | Bidirectional | serialosc protocol |

No MIDI. Communicates via OSC over UDP through the serialosc daemon, same as Arc.

## Protocol (OSC via serialosc)

### Input (device to host)
| Message | Description |
|---------|-------------|
| `/grid/key x y s` | Button at (x,y) pressed (s=1) or released (s=0) |

### Output (host to device)
| Message | Description |
|---------|-------------|
| `/grid/led/set x y l` | Set LED at (x,y) to level `l` (0-15) |
| `/grid/led/all l` | Set all LEDs to level `l` |
| `/grid/led/map x y d[0..7]` | Set 8x8 quad starting at (x,y) |
| `/grid/led/row x y d` | Set row of 8 LEDs |
| `/grid/led/col x y d` | Set column of 8 LEDs |
| `/grid/led/intensity l` | Set global intensity |

### Tilt (if equipped)
| Message | Description |
|---------|-------------|
| `/tilt n x y z` | Accelerometer data from sensor `n` |

## Programming
- **Lua on Norns** — primary use case, extensive `grid` module
- **Max/MSP** — serialosc objects
- **SuperCollider** — community libraries
- **Python** — `pymonome`
- **Any language** with UDP/OSC support

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

Current Norns scripts expect varibright (16 levels). Serial numbers starting with "m" + 7 digits indicate the newest mext protocol.
