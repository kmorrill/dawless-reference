# Analog Rytm MKII - MIDI CC Mapping (OS 1.72)

**Source**: Elektron Analog Rytm User Manual (ENG) for OS 1.72, Appendix C: MIDI
**Format**: Complete CC and NRPN mapping extracted from official documentation
**Data File**: `analog_rytm_mk2_midi_cc_map_os1.72.csv`

---

## Setup Requirements

**CRITICAL**: Before using CC/NRPN control, enable it on the device:

```
GLOBAL > MIDI CONFIG > RECEIVE CC/NRPN = ON
```

### MIDI Channel Configuration

The Rytm uses multiple MIDI channels for flexible routing:

- **TRACK 1–12**: Each drum track can have a dedicated MIDI channel
- **FX TRACK**: Separate channel for FX parameters
- **AUTO CHANNEL**: Controls whichever track is currently active on the device
- **PERF CHANNEL**: For performance mode parameters (Performance Parameters 1-12)

Configure channels in: `GLOBAL > MIDI CONFIG > CHANNELS`

---

## Quick Reference: Essential CCs

### Trig Parameters (C.1)
Core sequencer trig attributes:

| CC | Parameter | NRPN |
|----|-----------|------|
| **3** | Note | 3:0 |
| **4** | Velocity | 3:1 |
| **5** | Length | 3:2 |
| **11** | Synth Trig | 3:3 |
| **12** | Sample Trig | 3:4 |
| **13** | ENV Trig | 3:5 |
| **14** | LFO Trig | 3:6 |

### Track Control (C.3 - Common)

| CC | Parameter | NRPN |
|----|-----------|------|
| **15** | Track Machine Type | 1:103 |
| **92** | Active Scene | 1:104 |
| **93** | Track Solo (seq mute) | 1:102 |
| **94** | Track Mute (seq mute) | 1:101 |
| **95** | Track Level | 1:100 |

### Performance Macros (C.4)
12 assignable performance parameters (send on PERF CHANNEL):

| CC | Parameter | NRPN |
|----|-----------|------|
| **35** | Performance 1 | 0:0 |
| **36** | Performance 2 | 0:1 |
| **37** | Performance 3 | 0:2 |
| **39** | Performance 4 | 0:3 |
| **40** | Performance 5 | 0:4 |
| **41** | Performance 6 | 0:5 |
| **42** | Performance 7 | 0:6 |
| **43** | Performance 8 | 0:7 |
| **44** | Performance 9 | 0:8 |
| **45** | Performance 10 | 0:9 |
| **46** | Performance 11 | 0:10 |
| **47** | Performance 12 | 0:11 |

### Euclidean Sequencer (C.2)

| CC | Parameter | NRPN |
|----|-----------|------|
| **86** | Pulse Generator 1 | 3:8 |
| **87** | Pulse Generator 2 | 3:9 |
| **88** | Boolean Operator | 3:10 |
| **89** | Rotation Generator 1 | 3:11 |
| **90** | Rotation Generator 2 | 3:12 |
| **91** | Track Rotation | 3:13 |
| **117** | Euclidean on/off | 3:14 |

---

## Drum Track Sound Engine (C.5)

### SYNTH Parameters (CC 16-23)
**Important**: The meaning of CC 16-23 depends on the selected Machine type (see Machine Parameters section below)

| CC | Generic Name | NRPN |
|----|-------------|------|
| **16** | Synth Parameter 1 | 1:0 |
| **17** | Synth Parameter 2 | 1:1 |
| **18** | Synth Parameter 3 | 1:2 |
| **19** | Synth Parameter 4 | 1:3 |
| **20** | Synth Parameter 5 | 1:4 |
| **21** | Synth Parameter 6 | 1:5 |
| **22** | Synth Parameter 7 | 1:6 |
| **23** | Synth Parameter 8 | 1:7 |

### SAMPLE Parameters

| CC | Parameter | NRPN |
|----|-----------|------|
| **24** | Sample Tune | 1:8 |
| **25** | Sample Fine Tune | 1:9 |
| **26** | Bit Reduction | 1:10 |
| **27** | Sample Slot | 1:11 |
| **28** | Start | 1:12 |
| **29** | End | 1:13 |
| **30** | Loop | 1:14 |
| **31** | Level | 1:15 |

### FILTER Envelope & Parameters

| CC | Parameter | NRPN |
|----|-----------|------|
| **70** | Attack | 1:16 |
| **71** | Decay | 1:17 |
| **72** | Sustain | 1:18 |
| **73** | Release | 1:19 |
| **74** | Frequency | 1:20 |
| **75** | Resonance | 1:21 |
| **76** | Mode | 1:22 |
| **77** | Env Depth | 1:23 |

### AMP Envelope & Parameters

| CC | Parameter | NRPN |
|----|-----------|------|
| **78** | Attack | 1:24 |
| **79** | Hold | 1:25 |
| **80** | Decay | 1:26 |
| **81** | Overdrive | 1:27 |
| **82** | Delay Send | 1:28 |
| **83** | Reverb Send | 1:29 |
| **10** | Pan | 1:30 |
| **7** | Volume | 1:31 |

---

## LFO Parameters (C.6)

| CC | Parameter | NRPN | Notes |
|----|-----------|------|-------|
| **102** | Speed | 1:32 | |
| **103** | Multiplier | 1:33 | |
| **104** | Fade In/Out | 1:34 | |
| **105** | Destination | 1:35 | |
| **106** | Waveform | 1:36 | |
| **107** | Start Phase | 1:37 | |
| **108** | Trig Mode | 1:38 | |
| **109** (MSB) + **118** (LSB) | Depth | 1:39 | High-resolution 14-bit control |

---

## FX Track Parameters (C.7)

### DELAY

| CC | Parameter | NRPN |
|----|-----------|------|
| **16** | Time | 2:0 |
| **17** | Pingpong | 2:1 |
| **18** | Stereo Width | 2:2 |
| **19** | Feedback | 2:3 |
| **20** | HPF | 2:4 |
| **21** | LPF | 2:5 |
| **22** | Reverb Send | 2:6 |
| **23** | Mix Volume | 2:7 |

### REVERB

| CC | Parameter | NRPN |
|----|-----------|------|
| **24** | Predelay | 2:8 |
| **25** | Decay Time | 2:9 |
| **26** | Shelving Freq | 2:10 |
| **27** | Shelving Gain | 2:11 |
| **28** | HPF | 2:12 |
| **29** | LPF | 2:13 |
| **31** | Mix Volume | 2:15 |

### DISTORTION

| CC | Parameter | NRPN |
|----|-----------|------|
| **70** | Amount | 2:16 |
| **71** | Symmetry | 2:17 |
| **72** | Delay Overdrive | 2:18 |
| **76** | Delay Dist/Comp Routing (pre/post) | 2:22 |
| **77** | Reverb Dist/Comp Routing (pre/post) | 2:23 |

### COMPRESSOR

| CC | Parameter | NRPN |
|----|-----------|------|
| **78** | Threshold | 2:24 |
| **79** | Attack | 2:25 |
| **80** | Release | 2:26 |
| **81** | Makeup | 2:27 |
| **82** | Ratio | 2:28 |
| **83** | Sidechain EQ | 2:29 |
| **84** | Dry/Wet | 2:30 |
| **85** | Output | 2:31 |

---

## Machine-Specific Parameters (C.8)

**When a Machine is selected, CC 16-23 map to that machine's specific synth parameters.**

### Bass Drum (BD) Machines

#### BD ACOUSTIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Hold Time | 1:5 |
| 22 | Impact | 1:6 |
| 23 | Waveform | 1:7 |

#### BD CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Hold | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Sweep Depth | 1:5 |
| 22 | Waveform | 1:6 |
| 23 | Transient Tick | 1:7 |

#### BD FM

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | FM Amount | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | FM Sweep Time | 1:5 |
| 22 | FM Decay Time | 1:6 |
| 23 | FM Tune | 1:7 |

#### BD HARD

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Hold | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Sweep Depth | 1:5 |
| 22 | Waveform | 1:6 |
| 23 | Transient Tick | 1:7 |

#### BD PLASTIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Hold Time | 1:5 |
| 22 | VCO Click | 1:6 |
| 23 | Dust Level | 1:7 |

#### BD SHARP

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Hold Time | 1:5 |
| 22 | Tick Level | 1:6 |
| 23 | Waveform | 1:7 |

#### BD SILKY

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | Hold | 1:5 |
| 22 | VCO Click | 1:6 |
| 23 | Dust Level | 1:7 |

### Snare Drum (SD) Machines

#### SD ACOUSTIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Noise Decay | 1:3 |
| 20 | Hold Time | 1:4 |
| 21 | Noise Level | 1:5 |
| 22 | Impact | 1:6 |
| 23 | Sweep Depth | 1:7 |

#### SD CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Detune | 1:3 |
| 20 | Snap Amount | 1:4 |
| 21 | Noise Decay | 1:5 |
| 22 | Noise Level | 1:6 |
| 23 | Osc Balance | 1:7 |

#### SD FM

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | FM Tune | 1:3 |
| 20 | FM Decay Time | 1:4 |
| 21 | Noise Decay | 1:5 |
| 22 | Noise Level | 1:6 |
| 23 | FM Amount | 1:7 |

#### SD HARD

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Tick Level | 1:4 |
| 21 | Noise Decay | 1:5 |
| 22 | Noise Level | 1:6 |
| 23 | Sweep Time | 1:7 |

#### SD NATURAL

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Body Decay | 1:2 |
| 19 | Noise Decay | 1:3 |
| 20 | Noise LPF | 1:4 |
| 21 | Noise Balance | 1:5 |
| 22 | Noise Resonance | 1:6 |
| 23 | Noise HPF | 1:7 |

### Rim Shot (RS) Machines

#### RS CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune Osc 1 | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Osc Balance | 1:3 |
| 20 | Tune Osc 2 | 1:4 |
| 21 | Symmetry | 1:5 |
| 22 | Noise Level | 1:6 |
| 23 | Tick Level | 1:7 |

#### RS HARD

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Tick Level | 1:4 |
| 21 | Noise Level | 1:5 |
| 22 | Symmetry | 1:6 |
| 23 | Sweep Time | 1:7 |

### Clap (CP) Machines

#### CP CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Noise Tone | 1:1 |
| 18 | Noise Decay | 1:2 |
| 19 | Clap Number | 1:3 |
| 20 | Clap Rate | 1:4 |
| 21 | Noise Level | 1:5 |
| 22 | Random Claps | 1:6 |
| 23 | Clap Decay | 1:7 |

### Tom (BT) Machines

#### BT CLASSIC

| CC | Parameter | NRPN | Notes |
|----|-----------|------|-------|
| 16 | Level | 1:0 | |
| 17 | Tune | 1:1 | |
| 18 | Decay | 1:2 | |
| 19 | Sweep Depth | 1:3 | |
| 20 | Noise Level / Sweep Time | 1:4 | Dual mapping |
| 21 | Noise Decay / Snap Type | 1:5 | Dual mapping |
| 22 | Noise Level | 1:6 | |
| 23 | Noise Tone | 1:7 | |

### Cymbal (CY) Machines

#### CY CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Color | 1:3 |
| 20 | Tone | 1:4 |

#### CY METALLIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |
| 19 | Tone | 1:3 |
| 20 | Transient Decay | 1:4 |

#### CY RIDE

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Tail Decay | 1:2 |
| 19 | Hit Decay | 1:3 |
| 20 | Cymbal Type | 1:4 |
| 21 | Component 1 | 1:5 |
| 22 | Component 2 | 1:6 |
| 23 | Component 3 | 1:7 |

### Hi-Hat (CH/OH/HH) Machines

#### CH CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Color | 1:3 |

#### CH METALLIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |

#### OH CLASSIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Color | 1:3 |

#### OH METALLIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |

#### HH BASIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |
| 19 | Tone | 1:3 |
| 20 | Transient Decay | 1:4 |
| 21 | Osc Reset | 1:5 |

#### HH LAB

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune 1 | 1:1 |
| 18 | Decay Time | 1:2 |
| 19 | Tune 2 | 1:3 |
| 20 | Tune 3 | 1:3 |
| 21 | Tune 4 | 1:3 |
| 22 | Tune 5 | 1:4 |
| 23 | Tune 6 | 1:5 |

### Cowbell (CB) Machines

#### CB CLASSIC & METALLIC

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Tune | 1:1 |
| 18 | Decay Time | 1:2 |
| 19 | Detune | 1:3 |

### Synth (SY) Machines

#### SY CHIP

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | - |
| 17 | Tune | - |
| 18 | Decay | - |
| 19 | Waveform | - |
| 20 | Speed | - |
| 21 | Offset 2 | - |
| 22 | Offset 3 | - |
| 23 | Offset 4 | - |

#### SY DUAL VCO

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | - |
| 17 | Osc 1 Tune | - |
| 18 | Osc 1 Decay | - |
| 19 | Balance | - |
| 20 | Osc 2 Detune | - |
| 21 | Osc Config | - |
| 22 | Osc 2 Decay | - |
| 23 | Bend | - |

#### SY RAW

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | - |
| 17 | Tune | - |
| 18 | Decay | - |
| 19 | Noise Level | - |
| 20 | Osc 2 Detune | - |
| 21 | Waveform 1 | - |
| 22 | Waveform 2 | - |
| 23 | Balance | - |

### Utility (UT) Machines

#### UT IMPULSE

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | Attack | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Polarity | 1:3 |

#### UT NOISE

| CC | Parameter | NRPN |
|----|-----------|------|
| 16 | Level | 1:0 |
| 17 | LP Frequency | 1:1 |
| 18 | Decay | 1:2 |
| 19 | Sweep Depth | 1:3 |
| 20 | Sweep Time | 1:4 |
| 21 | LP Resonance | 1:5 |
| 22 | HP Frequency | 1:6 |
| 23 | Attack | 1:7 |

---

## Program Changes

**Pattern Selection**: Program Change 0-127 selects Pattern 1-128 (A01-H16)

**Enable in**: `GLOBAL > MIDI CONFIG > PRG CH RECEIVE = ON`

### Pattern Map
```
PC 0-15   = Bank A (A01-A16)
PC 16-31  = Bank B (B01-B16)
PC 32-47  = Bank C (C01-C16)
PC 48-63  = Bank D (D01-D16)
PC 64-79  = Bank E (E01-E16)
PC 80-95  = Bank F (F01-F16)
PC 96-111 = Bank G (G01-G16)
PC 112-127= Bank H (H01-H16)
```

---

## NRPN Format

NRPN messages use the standard MIDI format:
```
CC 99 (NRPN MSB): High byte
CC 98 (NRPN LSB): Low byte
CC 6 (Data Entry MSB): Value high byte
CC 38 (Data Entry LSB): Value low byte (for 14-bit params)
```

All NRPN mappings are included in the CSV file for programmatic access.

---

## Notes & Caveats

1. **OS Version**: This mapping is for OS 1.72. Parameter assignments may differ in other firmware versions.
2. **Device Variant**: This documentation is for the **Analog Rytm MKII**. The MKI may have differences.
3. **High-Resolution Parameters**: LFO Depth (CC 109 MSB + CC 118 LSB) uses 14-bit resolution for fine control.
4. **Overlapping CCs**: Note that CC 16-23 serve multiple purposes depending on context:
   - On TRACK channels: Machine-specific synth parameters
   - On FX channel: Delay/Reverb parameters
5. **Dual Mappings**: Some machine parameters (e.g., BT CLASSIC CC 20/21) have dual purposes noted in the CSV.
6. **Channel Routing**: Always verify which MIDI channel you're sending to - track-specific, auto, or performance channel.

---

## CSV Data Structure

The included CSV file contains all mappings with the following columns:

- `page_idx`: Manual page reference
- `section`: Section identifier (C.1-C.8)
- `subsection`: Parameter category (TRIG, SYNTH, FILTER, etc.)
- `machine`: Specific machine type (or empty for common params)
- `parameter`: Parameter name
- `cc_msb`: MIDI CC number (or MSB for 14-bit)
- `cc_lsb`: LSB for 14-bit parameters (or empty)
- `nrpn_msb`: NRPN high byte
- `nrpn_lsb`: NRPN low byte
- `raw_line`: Original manual text

---

## Additional Resources

- Full device manual: `device-manual.md`
- MIDI overview: `midi-reference.md`
- Programming options: `library-options.md`
