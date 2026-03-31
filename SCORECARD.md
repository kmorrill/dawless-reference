# Documentation Completeness Scorecard

Tracks what's documented for each device and what's missing. Used to prioritize research and writing.

## Column Definitions

| Column | What it covers |
|---|---|
| **Overview** | Device identity, specs, voice architecture |
| **Conn.** | Every physical port with direction and jack type |
| **CC Map (deep)** | Full CC table including per-channel, per-engine, and per-mode variations |
| **PC** | Preset/pattern selection protocol (bank select + PC) |
| **Chan** | Default channels, per-track routing, MPE zones, reconfigurability |
| **Sync** | Clock send/receive, ppqn, transport, master/follower |
| **Controls** | Physical knob/slider/button → function mapping |
| **SysEx/USB** | SysEx, NRPN, and any non-CC USB control protocols (Overbridge, Haken, serialosc, etc.) |
| **Limits** | What can't be controlled externally |
| **Software** | Companion apps, editors, firmware tools |
| **impl.md** | Separate midi-implementation.md file |
| **Formats** | Proprietary file format docs |
| **PDFs** | Official manuals included/referenced |
| **3P Repos** | Useful 3rd-party GitHub repos identified and linked |
| **Local Mine** | Info mined from ~/Documents repos |

## Legend

- ✅ Full — comprehensive, no known gaps
- 🟡 Partial — some coverage, known gaps remain
- ❌ Missing — not documented
- ➖ N/A — category doesn't apply to this device

## Scorecard

| Device | Overview | Conn. | CC Deep | PC | Chan | Sync | Controls | SysEx/USB | Limits | Software | impl.md | Formats | PDFs | 3P Repos | Local Mine |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **TE OP-XY** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 🟡 | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ✅ | ✅ |
| **Roland S-1** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ✅ | 🟡 |
| **Roland E-4** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ✅ | 🟡 |
| **Roland J-6** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ➖ | 🟡 |
| **Roland T-8** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ✅ | 🟡 |
| **MiniFreak** | ✅ | ✅ | ✅ | ✅ | 🟡 | 🟡 | ✅ | 🟡 | ✅ | ✅ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **MicroFreak** | ✅ | ✅ | ✅ | 🟡 | 🟡 | 🟡 | ✅ | ❌ | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **Rytm MK2** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ |
| **Osmose** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **KeyStep MK2** | ✅ | ✅ | 🟡 | 🟡 | 🟡 | ✅ | ✅ | ❌ | ✅ | 🟡 | ❌ | ➖ | ❌ | ➖ | ❌ |
| **NDLR** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ➖ | ✅ | ❌ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **OP-Z** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **TX-6** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **TP-7** | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ | ➖ | ❌ | ✅ | ❌ |
| **Polyend Synth** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 🟡 | ✅ | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **ROLI Blocks** | ✅ | ✅ | 🟡 | ❌ | ✅ | ➖ | ✅ | ✅ | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **ROLI Rise 2** | ✅ | ✅ | 🟡 | ❌ | ✅ | ➖ | ❌ | ✅ | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **Pocket Ops** | 🟡 | ✅ | ➖ | ➖ | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Norns** | ✅ | ✅ | ➖ | ➖ | 🟡 | ➖ | 🟡 | 🟡 | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **Grid** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ➖ | ✅ | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **Arc** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ➖ | ✅ | ✅ | 🟡 | ❌ | ➖ | ❌ | ✅ | ❌ |
| **CM-15** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ❌ | ➖ | 🟡 | ➖ | ❌ | ➖ | ❌ | ❌ | ❌ |

## Priority Queue (updated 2026-03-31)

### Completed
All P1, P2, and P3 devices have been researched and documented. Major improvements:
- **Rytm MK2**: Full per-engine CC/NRPN tables, SysEx protocol, 3P repos, local repo mining
- **TX-6**: Full CC map (9 channels), PC, sync, BLE MIDI, 3P repos
- **OP-XY**: PC, sync, 3P repos, deep local mine
- **Polyend Synth**: Full rewrite — 87 controls, 10 engines, CC map, arp, sequencer
- **TP-7**: Bidirectional CC map, physical controls, 3P repos
- **Norns/Grid/Arc**: Full rewrites with protocol details, limitations, 3P repos
- **ROLI Blocks/Rise 2**: SysEx protocol, physical controls, 3P repos
- **Osmose**: PC (fw 2.0), sync, Haken Editor protocol, physical controls, 3P repos
- **NDLR**: Complete CC table (~50 CCs), 3P repos, limitations
- **OP-Z**: Full CC table, midi.json format, SysEx, step components, 25+ 3P repos
- **AIRA Compacts**: Physical controls for all 4 devices, 3P repos
- **Arturia**: Physical controls + CC maps for MiniFreak (41 CCs), MicroFreak (21 CCs), KeyStep mk2

### Remaining gaps

| Device | Remaining gaps |
|---|---|
| **OP-XY** | SysEx/USB protocol, companion software docs |
| **KeyStep MK2** | SysEx, deeper PC investigation |
| **Pocket Ops / CM-15** | Minimal MIDI surface — diminishing returns |
| **Various** | PDFs, impl.md files, companion software docs |
