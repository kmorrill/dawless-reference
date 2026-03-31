# dawless-reference

Hardware documentation for a dawless setup. Every device in one place — MIDI maps, control surfaces, connectivity, formats, limitations. Designed to be pulled in as a git submodule by project repos that need device info.

## Usage as submodule

```bash
git submodule add https://github.com/kevinmorrill/dawless-reference.git docs/dawless-reference
```

## Devices

### Teenage Engineering
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [OP-XY](devices/teenage-engineering/op-xy/) | 16ch send/receive | USB-C device | 8 synth + 8 aux tracks, 8 engines |
| [OP-Z](devices/teenage-engineering/op-z/) | 16ch send/receive | USB-C device | 16 tracks, parameter locks |
| [TX-6](devices/teenage-engineering/tx-6/) | CC control | USB-C host/device | 6-ch mixer, audio interface |
| [TP-7](devices/teenage-engineering/tp-7/) | — | USB-C | Field recorder, tape machine |
| [CM-15](devices/teenage-engineering/cm-15/) | — | USB-C | Submixer, 4 stereo in |
| [Pocket Operators](devices/teenage-engineering/pocket-operators/) | Sync (3.5mm) | — | Various models |

### Roland AIRA Compact
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [S-1](devices/roland/s-1/) | DIN + USB | USB-C | Tweak synth, AIRA Link |
| [E-4](devices/roland/e-4/) | DIN + USB | USB-C | Voice tweaker |
| [J-6](devices/roland/j-6/) | DIN + USB | USB-C | Chord synth |
| [T-8](devices/roland/t-8/) | DIN + USB | USB-C | Beat machine |

### Arturia
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [MiniFreak](devices/arturia/minifreak/) | DIN + USB | USB-B | Hybrid poly, 2 osc, 6 voice |
| [MicroFreak](devices/arturia/microfreak/) | DIN + USB | USB-B | Hybrid mono/para, touch keyboard |
| [KeyStep MK II](devices/arturia/keystep-mk2/) | DIN + USB + CV | USB-B | Controller, seq, arp |

### Expressive E
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [Osmose](devices/expressive-e/osmose/) | DIN + USB | USB-B | MPE, 49 keys, per-note expression |

### ROLI
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [Blocks](devices/roli/blocks/) | — | USB-C / BLE | MPE, Lightpad + Seaboard |
| [Rise 2](devices/roli/rise-2/) | — | USB-C / BLE | MPE, 49 keys, 5D touch |

### Monome
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [Norns](devices/monome/norns/) | — | USB host | Lua scripting, SuperCollider |
| [Grid](devices/monome/grid/) | — | USB (serialosc) | 128/256 button matrix |
| [Arc](devices/monome/arc/) | — | USB (serialosc) | 2/4 high-res encoders, LED rings |

### Polyend
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [Synth](devices/polyend/synth/) | DIN + USB | USB-C | 6-voice poly, 48 presets |

### Elektron
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [Analog Rytm MKII](devices/elektron/analog-rytm-mk2/) | DIN in/out/thru | USB-B (Overbridge) | 12-voice analog drums, sampling, p-locks |

### Conductive Labs
| Device | MIDI | USB | Notes |
|--------|------|-----|-------|
| [NDLR](devices/conductive-labs/ndlr/) | DIN (2 out + thru) | USB | Harmonic conductor, 4 parts |

## Per-device documentation standard

Each device folder should contain:
- **README.md** — Overview, specs, connectivity, physical controls
- **midi-implementation.md** — Channels, CCs, NRPN, sysex, MPE details
- **limitations.md** — What can't be controlled externally
- **formats/** — Any proprietary file formats (presets, projects, loops)
- **pdfs/** — Official manuals and spec sheets
