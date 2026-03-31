# Roland AIRA Compact S-1 — MIDI Reference (for coding agent)
Source: **S-1 Owner’s Manual (Eng) — MIDI implementation chart + control change list** (Version **1.02**, dated **Apr 18, 2023**).

This document is intended to be *implementation-ready* for a WebMIDI / DAW / controller app.

---

## 0) Connection + Ports
The S-1 supports MIDI over:
- **USB-MIDI** (USB Type‑C)
- **TRS MIDI IN/OUT** (3.5mm TRS)

---

## 1) MIDI Channels
### 1.1 Default channels
- **Synth channel default:** **3**
- **Program Change channel default:** **16**

### 1.2 Changeable range
- Synth channel can be set to **1–16**
- Program Change channel can be set to **1–16**

### 1.3 Device setting (menu items)
- `CH` — MIDI transmit/receive channel (1–16)
- `Pc.Ch` — Program Change channel (1–16)
- `txPc` — Transmit program change on pattern change (OFF/On)
- `rxPc` — Receive program change to change patterns (OFF/On)

---

## 2) Supported MIDI Message Types (RX/TX)

> **Key:** TX = transmitted by S‑1, RX = recognized by S‑1

### 2.1 Notes
- **Note Number (True Voice):** 0–127  
  - TX: Yes  
  - RX: Yes
- **Velocity:**
  - **Note On:** TX Yes / RX Yes
  - **Note Off:** TX No / RX No  
    - Practical implication: send Note Off as **Note On with velocity 0** (common MIDI practice).

### 2.2 Aftertouch
- **Polyphonic key pressure:** TX No / RX No  
- **Channel pressure:** TX No / RX No

### 2.3 Pitch Bend
- **Pitch Bend:** TX No / RX Yes

### 2.4 Control Change (CC)
- TX: **Some** (see chart list)
- RX: **Yes** (for all CCs in the supported list)
- See section **3** for the full CC map.

### 2.5 Program Change (pattern change)
- **Program Change (True Number):** 0–63  
  - TX: Yes  
  - RX: Yes  
- Notes:
  - Pattern change via Program Change is gated by menu settings `txPc` / `rxPc` and channel `Pc.Ch`.

### 2.6 System Exclusive (SysEx)
- **SysEx:** TX No / RX No

### 2.7 System Common
- **Song Position Pointer:** TX No / RX No
- **Song Select:** TX No / RX No
- **Tune Request:** TX No / RX No

### 2.8 System Real-Time (sync/transport)
- **Clock:** TX Yes / RX Yes
- **Start:** TX Yes / RX Yes
- **Continue:** TX No / RX No
- **Stop:** TX Yes / RX Yes

### 2.9 “Aux” / Channel Mode / Other
- **All Sound Off:** TX Yes / RX Yes
- **Reset All Controllers:** TX No / RX Yes
- **All Notes Off:** TX No / RX Yes
- **Omni Mode Off/On:** TX No / RX No
- **Mono Mode On:** TX No / RX No
- **Poly Mode On:** TX No / RX No
- **Active Sensing:** TX Yes / RX Yes
- **System Reset:** TX No / RX No

---

## 3) Full Control Change (CC) List (Supported by S‑1)

### 3.0 Enumerated CC Value Maps (quick lookup)
- (CC12) LFO WAVE FORM
  - 0: Sawtooth (rising saw)
  - 1: Inverted sawtooth (falling saw)
  - 2: Triangle
  - 3: Square
  - 4: RND (Random)
  - 5: NOISE
- (CC16) PWM Source
  - 0: Envelope (PWM changes over time using the envelope generator)
  - 1: Manual / fixed value (PWM is a fixed setting)
  - 2: LFO (PWM changes over time using the LFO)
- (CC22) Sub Oct Type
  - 1: Two octaves down, asymmetrical pulse (narrower pulse width)
  - 2: Two octaves down, symmetrical
  - 3: One octave down, symmetrical
- (CC28) Amp Mode
  - 0: Gate (steady level while key is held; no ADSR shaping)
  - 1: Envelope (amp follows the ADSR envelope)
- (CC29) Envelope Trigger
  - 0: LFO (retrigger envelope in sync with LFO cycle while held)
  - 1: Gate (trigger on new note; legato may not retrigger)
  - 2: Gate + Trig (retrigger on every key press)
- (CC31) Portamento Mode
  - 0: Off (no glide)
  - 1: On (always glide)
  - 2: Auto (glide only on legato)
- (CC78) Noise Mode
  - 0: Pink noise
  - 1: White noise
- (CC79) LFO Mode
  - 0: Normal (regular LFO behavior)
  - 1: Fast (extra-fast modulation)
- (CC80) Poly Mode
  - 0: Mono (single note)
  - 1: Unison (stacked voices on one note)
  - 2: Poly (up to 4-note polyphony)
  - 3: Chord mode (one key plays a chord by adding extra voices)
- (CC93) Chorus Type
  - 0: Off
  - 1: Type 1 (standard chorus)
  - 2: Type 2 (faster modulation chorus)
  - 3: Type 3 (fast rotary-speaker-like modulation)
  - 4: Type 4 (more relaxed / slower modulation chorus)
- (CC105) LFO Key Trigger
  - 0: Off (LFO does not reset when a note starts)
  - 1: On (LFO resets when a note starts)
- (CC106) LFO Sync
  - 0: Off (rate is free-running)
  - 1: On (rate locks to tempo and uses note-length values)
- (CC107) OSC Draw Switch
  - 0: Off (normal square wave)
  - 1: Step (stepped “staircase” drawn waveform)
  - 2: Slope (sloped drawn waveform)

### 3.1 Performance / General CCs
| CC# | Name | Notes |
|---:|---|---|
| 1 | Modulation Wheel | Standard CC1. Used by S‑1 to apply its internal LFO sine output as modulation. |
| 10 | Pan | Standard CC10. |
| 11 | Expression | Standard CC11. |
| 64 | Damper Pedal | Sustain. |
| 65 | Portamento | Portamento on/off (standard CC65). |

### 3.2 LFO
| CC# | Parameter |
|---:|---|
| 3 | **LFO RATE** (LFO [RATE] knob) |
| 12 | **LFO WAVE FORM** (LFO [WAVE FORM] knob) |
| 79 | **LFO MODE** (menu: `LFO.N`; also [SHIFT]+LFO RATE knob) |
| 106 | **LFO SYNC** (menu: `LFO.S`; also [SHIFT]+LFO WAVE FORM knob) |
| 105 | **LFO KEY TRIGGER** (menu: `LFO.K`) |
| 17 | **LFO MODULATION DEPTH** (menu: `Nod.d`) |

### 3.3 Oscillator / Mixer
| CC# | Parameter |
|---:|---|
| 13 | OSC LFO (OSCILLATOR [LFO] knob; LFO→pitch amount) |
| 14 | OSC RANGE (OSCILLATOR [RANGE] knob) |
| 76 | FINE TUNE ([SHIFT]+OSC RANGE knob) |
| 19 | OSC LEVEL (square / OSC DRAW level depending on mode) |
| 20 | OSC LEVEL (saw level) |
| 21 | OSC SUB LEVEL |
| 22 | SUB OCT TYPE |
| 23 | OSC NOISE LEVEL (when Riser Mode is OFF) |
| 15 | OSC PULSE WIDTH ([SHIFT]+PWM DEPTH / [SHIFT]+square level knob) |
| 16 | OSC PWM SOURCE ([SHIFT]+PWM SRC) |
| 78 | NOISE MODE (menu: `nSNd`; also [SHIFT]+NOISE knob when Riser Mode is OFF) |
| 18 | OSC BEND SENS (menu: `bnd.o`) |

### 3.4 Filter
| CC# | Parameter |
|---:|---|
| 74 | FILTER FREQUENCY (cutoff) |
| 71 | FILTER RESONANCE |
| 25 | FILTER LFO depth |
| 24 | FILTER ENVELOPE depth |
| 26 | FILTER KEYBOARD FOLLOW ([SHIFT]+FILTER KYBD / [SHIFT]+FILTER ENV knob) |
| 27 | FILTER BEND SENS (menu: `bnd.F`) |

### 3.5 Amp / Envelope
| CC# | Parameter |
|---:|---|
| 28 | AMP ENVELOPE MODE SW ([SHIFT]+AMP; Gate vs Env) |
| 29 | ENV TRIGGER MODE ([SHIFT]+ENV TRG) |
| 73 | ENV ATTACK |
| 75 | ENV DECAY |
| 30 | ENV SUSTAIN |
| 72 | ENV RELEASE |

### 3.6 Poly / Chord / Voice
| CC# | Parameter |
|---:|---|
| 80 | POLY MODE (Mono/Unison/Poly/Chord) |
| 81 | CHORD VOICE 2 SW |
| 82 | CHORD VOICE 3 SW |
| 83 | CHORD VOICE 4 SW |
| 85 | CHORD VOICE 2 KEY SHIFT |
| 86 | CHORD VOICE 3 KEY SHIFT |
| 87 | CHORD VOICE 4 KEY SHIFT |

### 3.7 Transpose / Portamento
| CC# | Parameter |
|---:|---|
| 5 | PORTAMENTO TIME |
| 31 | PORTAMENTO MODE ([SHIFT]+PORTA ON; Off/On/Auto) |
| 77 | TRANSPOSE SW ([SHIFT]+STEP (Key Transpose) button) |

> Note: **Key transpose value** itself is a menu/value operation on the device; MIDI provides **Transpose SW (CC77)**, not the numeric transpose amount.

### 3.8 Effects (Delay / Reverb / Chorus)
| CC# | Parameter |
|---:|---|
| 92 | DELAY LEVEL |
| 90 | DELAY TIME |
| 91 | REVERB LEVEL |
| 89 | REVERB TIME |
| 93 | CHORUS type (menu: `Cho`) |

### 3.9 OSC DRAW / OSC CHOP (Sound shaping)
| CC# | Parameter |
|---:|---|
| 107 | OSC DRAW SW (Off / Step / Slope) |
| 102 | OSC DRAW MULTIPLY |
| 103 | OSC CHOP OVERTONE |
| 104 | OSC CHOP COMB |

> Important limitation: OSC DRAW “Form” (the 16 step levels) and OSC CHOP step patterns are edited on-device and are **not** exposed as CCs.

---

## 4) Knob-to-CC Mapping (including SHIFT variants)
This helps an app mirror the hardware UI behavior.

### 4.1 LFO section
- LFO RATE knob → CC3  
  - SHIFT+LFO RATE → **LFO MODE** (CC79)
- LFO WAVE FORM knob → CC12  
  - SHIFT+LFO WAVE FORM → **LFO SYNC** (CC106)

### 4.2 OSCILLATOR section
- RANGE knob → CC14  
  - SHIFT+RANGE → **FINE TUNE** (CC76)
- OSC LFO knob → CC13  
  - SHIFT+OSC LFO → **OSC CHOP OVERTONE** (CC103)
- Square level knob → CC19  
  - SHIFT+Square level knob → **Pulse Width** (CC15) *when OSC DRAW SW is OFF*  
  - SHIFT+Square level knob → **OSC DRAW MULTIPLY** (CC102) *when OSC DRAW SW is STEP/SLOPE*
- Saw level knob → CC20  
  - SHIFT+Saw level knob → **OSC DRAW SW** (CC107)
- SUB knob → CC21  
  - SHIFT+SUB → **OSC CHOP COMB** (CC104)
- NOISE knob → CC23 (when Riser Mode OFF)  
  - SHIFT+NOISE → **NOISE MODE** (CC78) (when Riser Mode OFF)

### 4.3 FILTER section
- FREQ knob → CC74
- RESO knob → CC71
- FILTER LFO knob → CC25
- FILTER ENV knob → CC24  
  - SHIFT+FILTER ENV knob → **FILTER KEYBOARD FOLLOW** (CC26)

### 4.4 ENV section
- ATTACK → CC73
- DECAY → CC75
- SUSTAIN → CC30
- RELEASE → CC72

### 4.5 EFX section
- DELAY knob → CC92  
  - SHIFT+DELAY → **DELAY TIME** (CC90)
- REVERB knob → CC91  
  - SHIFT+REVERB → **REVERB TIME** (CC89)

---

## 5) Pattern / Sequencer Notes Relevant to MIDI Apps
- The S‑1 can record and play back “motions” (parameter changes) in patterns.
- Motions can include **MIDI CC and Pitch Bend** changes that the unit receives.
- There is a per‑pattern cap: **up to 8 parameters** of motion + pitch bend.
- MIDI CC and Pitch Bend can only be recorded to motion when using **real‑time recording**.

(These constraints matter if you build a sequencer/app that tries to “parameter-lock” lots of CCs.)

---

## 6) Sync / Transport Configuration (device settings)
Menu item `SYnC` (MIDI Clock Sync) selects the sync source:
- `AUtO` (Auto)
- `Int` (Internal)
- `NiDi` (MIDI only)
- `USb` (USB MIDI only)

Also:
- `thru` — MIDI Thru (OFF/On)
- `S.cLk` — Sync clocks per beat (1,2,3,4,6,8,12,24)

---

## 7) Non‑supported / Missing for remote control (important constraints)
- No SysEx (no patch dump/restore via SysEx).
- No aftertouch.
- No Song Position Pointer / Song Select / Continue.
- Channel mode messages (Omni/Mono/Poly) are not recognized.
- Many on-device functions are not CC-addressable (e.g., arpeggiator configuration, OSC DRAW Form steps, OSC CHOP step masks, deep delay/reverb settings beyond the CCs listed).

---

## 8) Quick “What to Send” Cheat Sheet
### 8.1 Play notes
- Send Note On/Off on the **synth channel** (default 3).

### 8.2 Common live sound design CCs
- Cutoff: CC74
- Resonance: CC71
- Env depth: CC24
- LFO depth (filter): CC25
- LFO rate: CC3
- Osc LFO amount: CC13
- ADSR: CC73/75/30/72
- Delay/Reverb: CC92/90 and CC91/89

### 8.3 Poly/Chord automation
- Mode: CC80
- Chord voice enables: CC81–83
- Chord voice transposes: CC85–87

---

## 9) Version / Provenance
- Manual: S‑1 Owner’s Manual (Eng)
- MIDI chart date: Apr 18, 2023
- Firmware/doc version referenced: 1.02
