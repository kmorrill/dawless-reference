# Roland AIRA Compact T-8 — External MIDI Control Reference (MIDI IN)

Source: **T-8 Owner’s Manual v1.02**

---

## 0) Connections (how MIDI gets in)
- **TRS MIDI IN** (stereo mini) via TRS↔TRS or TRS↔5-pin MIDI adapter cables (Roland lists BCC-* or BMIDI-* cables).
- **USB MIDI** over USB-C (class compliant; also carries USB audio).
- **MIDI Thru (DIN/TRS path):** menu item `thrv` controls whether messages arriving at **MIDI IN** are echoed out **MIDI OUT** (`On` default).

---

## 1) MIDI channels (Rx)
The T-8 splits MIDI into **Rhythm** and **Bass**, plus a dedicated **Program Change channel**.

### Defaults
- **Bass channel:** 2
- **Rhythm channel:** 10
- **Program Change channel:** 16

### Configurable (per menu)
- `rYCh` — Rhythm MIDI Channel: **1-16 or OFF**
- `b0Ch` — Bass MIDI Channel: **1-16 or OFF**
- `Pc.Ch` — Program Change Channel: **1-16**

---

## 2) Note triggers (Rx) — what notes do what

### 2.1 Rhythm instruments (drum triggers)
Send **Note On/Off** on the **Rhythm MIDI Channel**.

| Instrument | Rx Note Number(s) |
|---|---:|
| Bass Drum | 35, 36 |
| Snare Drum | 38, 40 |
| Hand Clap | 48, 50 |
| Tom | 45, 47 |
| Closed Hi-hat | 42, 44 |
| Open Hi-hat | 46 |

Manual also shows the *Tx* note numbers (what the T-8 itself transmits), but the **Rx list above is what you need to trigger it externally**.

**Velocity:** Note On velocity is supported (recognized).

---

### 2.2 Bass notes
Send **Note On/Off** on the **Bass MIDI Channel**.

- **Recognized note range:** **12-96** (MIDI note numbers).
  - That corresponds to **C0 (12)** through **C7 (96)** under the common `60 = C4` convention.

**Velocity:** Note On velocity is supported (recognized).

---

## 3) Pattern changes (Program Change)

### Receive (Rx)
- **Program Change:** supported
- **Valid PC numbers:** **0-63** (64 values)

### Control whether Program Change works
- `rXPc` — **RX Program Change**: `On/Off` (whether incoming PC changes the pattern)
- `Pc.Ch` — **Program Change Channel**: `1-16`

### Transmit (Tx)
- `tXPc` — **TX Program Change**: `On/Off` (whether the T-8 sends PC when you change patterns)

> Note: The manual specifies the PC range (`0-63`) but does not spell out the exact mapping from PC number → bank/pattern label; it is intended to cover the 64 user patterns.

---

## 4) Sync / transport (clock control)

### Real-time messages
Recognized (Rx):
- **MIDI Clock** ✔
- **Start** ✔
- **Continue** ✔
- **Stop** ✔

Transmitted (Tx):
- **MIDI Clock** ✔
- **Start** ✔
- **Stop** ✔
- **Continue** ✘

### Selecting where clock comes from (menu)
`SYnC` — MIDI Clock Sync:
- `AUTO` (accept incoming clocks),
- `Int` (internal clock),
- `MIDI` (only MIDI IN),
- `USB` (only USB MIDI)

---

## 5) CC, Pitch Bend, Aftertouch, SysEx (what you *cannot* do)

Per the MIDI Implementation Chart:

### Not supported (Rx or Tx)
- **Control Change (CC):** ✘ (no CC parameter control)
- **Pitch Bend:** ✘
- **Aftertouch (poly/channel):** ✘
- **System Exclusive (SysEx):** ✘
- **Song Position / Song Select / Tune Request:** ✘

So: **you cannot remote-control the T-8 knobs/parameters via MIDI CC or SysEx** according to the manual.

---

## 6) Other MIDI/system messages

Supported (as listed):
- **All Sound Off:** Tx ✔ / Rx ✔
- **All Notes Off:** Tx ✘ / Rx ✔
- **Active Sensing:** Tx ✔ / Rx ✔

(“Reset All Controllers” and most Omni/Mono/Poly mode messages are not supported.)

---

## 7) Practical “how to drive it” recipe
1. Set `rYCh` to the channel your sequencer uses for drums (or keep default **10**).
2. Set `b0Ch` to the channel your sequencer uses for bass notes (or keep default **2**).
3. Send drum notes using the **Rx Note Numbers** table above.
4. Send bass notes in the **12-96** range on the bass channel.
5. If you want pattern selection, enable `rXPc` and send **PC 0-63** on `Pc.Ch`.
6. If you want the T-8 to follow your DAW clock/transport, set `SYnC` to `MIDI` or `USB` (or `AUTO`) and send clock + start/stop.

---
