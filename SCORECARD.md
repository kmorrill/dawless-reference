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
| **TE OP-XY** | ✅ | ✅ | 🟡 | ❌ | ✅ | ❌ | 🟡 | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | 🟡 |
| **Roland S-1** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ❌ | 🟡 |
| **Roland E-4** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ❌ | 🟡 |
| **Roland J-6** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ❌ | 🟡 |
| **Roland T-8** | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ❌ | ✅ | ➖ | ✅ | ❌ | 🟡 |
| **MiniFreak** | ✅ | ✅ | ✅ | ✅ | 🟡 | 🟡 | ❌ | 🟡 | ✅ | ✅ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **MicroFreak** | ✅ | ✅ | ✅ | 🟡 | 🟡 | 🟡 | ❌ | ❌ | ✅ | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Rytm MK2** | ✅ | ✅ | 🟡 | 🟡 | ✅ | 🟡 | ❌ | ❌ | ✅ | ✅ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Osmose** | ✅ | ✅ | 🟡 | ❌ | ✅ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **KeyStep MK2** | ✅ | ✅ | 🟡 | ❌ | 🟡 | ✅ | ❌ | ❌ | 🟡 | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **NDLR** | 🟡 | ✅ | 🟡 | 🟡 | 🟡 | 🟡 | ❌ | 🟡 | ❌ | ❌ | ❌ | ➖ | ❌ | ❌ | 🟡 |
| **OP-Z** | 🟡 | ✅ | 🟡 | 🟡 | ✅ | 🟡 | ❌ | ❌ | 🟡 | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **TX-6** | 🟡 | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🟡 | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **TP-7** | 🟡 | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | 🟡 | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Polyend Synth** | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ❌ | 🟡 | ❌ | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **ROLI Blocks** | 🟡 | ✅ | 🟡 | ❌ | ✅ | ➖ | ❌ | ❌ | 🟡 | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **ROLI Rise 2** | 🟡 | ✅ | 🟡 | ❌ | ✅ | ➖ | ❌ | ❌ | 🟡 | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Pocket Ops** | 🟡 | ✅ | ➖ | ➖ | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Norns** | ✅ | ✅ | ➖ | ➖ | 🟡 | ➖ | 🟡 | 🟡 | ❌ | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Grid** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ➖ | ✅ | ❌ | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **Arc** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ➖ | ✅ | ❌ | 🟡 | ❌ | ➖ | ❌ | ❌ | ❌ |
| **CM-15** | ✅ | ✅ | ➖ | ➖ | ➖ | ➖ | ❌ | ➖ | 🟡 | ➖ | ❌ | ➖ | ❌ | ❌ | ❌ |

## Priority Queue

### P1 — High-value targets

| # | Device | Key gaps | Approach |
|---|---|---|---|
| 1 | **Elektron Analog Rytm MK2** | Full per-engine CC map, SysEx/NRPN, Overbridge protocol, 3P repos | Mine local rytm repo (believed open-sourced), web research |
| 2 | **TE TX-6** | CC map, channel routing, sync, USB host MIDI protocol, 3P repos | Web research (TE guides, community) |
| 3 | **TE OP-XY** | PC behavior, sync/ppqn, SysEx/USB control, 3P repos, deeper local mine | Mine local repos (op-xy-live, op-xy-vibing, xy-format, xy-remix), web research |
| 4 | **Polyend Synth** | Overview, connectivity, controls, limitations | Web research |
| 5 | **TP-7** | Nearly everything — CC, PC, sync, controls, USB protocol | Web research (TE guides) |

### P2 — Medium value

| # | Device | Key gaps |
|---|---|---|
| 6 | **Monome Norns** | Limitations, 3P repos (community scripts, engines) |
| 7 | **Monome Grid** | Limitations, 3P repos |
| 8 | **Monome Arc** | Limitations, 3P repos |
| 9 | **ROLI Rise 2** | SysEx, controls, 3P repos |
| 10 | **ROLI Blocks** | SysEx, controls, 3P repos |
| 11 | **Osmose** | PC, sync, Haken Editor USB protocol, 3P repos |
| 12 | **KeyStep MK2** | PC, controls, SysEx |
| 13 | **MiniFreak/MicroFreak** | Controls map, PDFs |

### P3 — Lower urgency

| # | Device | Key gaps |
|---|---|---|
| 14 | **NDLR** | CC summary in README, limitations (reference device, not owned) |
| 15 | **OP-Z** | Full per-track CC table, midi.json format, 3P repos |
| 16 | **Roland AIRA (S-1/E-4/J-6/T-8)** | Controls map, 3P repos (already best-documented tier) |
| 17 | **Pocket Ops / CM-15** | Minimal MIDI surface — diminishing returns |
