# Teenage Engineering OP-Z

## Overview
- **Type**: 16-track sequencer / synthesizer / visual controller
- **Tracks**: 16 (4 drum, 4 synth, 2 FX, tape, master, perform, module, lights, motion)
- **Sequencer**: 16 steps per pattern with parameter locks, step components, pattern chaining
- **Polyphony**: Varies by track (1-4 notes per step depending on track)
- **Screen**: None built-in (uses iOS/Android app for visual interface)
- **Sampling**: Built-in microphone, OP-1 format .aif samples

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-C | In/Out | MIDI device mode (class-compliant), audio, charging |
| Bluetooth LE | Out | MIDI, connects to OP-Z app |
| 3.5mm line out | Out | Stereo audio |

No DIN MIDI. USB-C MIDI is class-compliant.

---

## Tracks (1-16)

### Drum Group (Tracks 1-4)
Sample-based sounds, 24 sounds per kit across the keyboard, two-note polyphony per step. Compatible with OP-1 drum format.

| Track | Name | Default Ch | Polyphony |
|-------|------|-----------|-----------|
| 1 | Kick | 1 | 2 |
| 2 | Snare | 2 | 2 |
| 3 | Perc | 3 | 2 |
| 4 | Sample | 4 | 2 |

### Synth Group (Tracks 5-8)
Use any available synth engine or load OP-1 format sample sounds.

| Track | Name | Default Ch | Polyphony | Notes |
|-------|------|-----------|-----------|-------|
| 5 | Bass | 5 | 1 (mono) | Main source for master track transpose analysis |
| 6 | Lead | 6 | 3 | |
| 7 | Arp | 7 | 1 (mono) | Arp controls replace LFO page |
| 8 | Chord | 8 | 4 | Harmony/pad role |

### Control Tracks (9-16)

| Track | Name | Default Ch | Function |
|-------|------|-----------|----------|
| 9 | FX1 | 9 | Send effects control |
| 10 | FX2 | 10 | Send effects control |
| 11 | Tape | 11 | Audio buffer looping effects |
| 12 | Master | 12 | Transpose, harmonic chord progressions |
| 13 | Perform | 13 | Punch-in effects across all tracks |
| 14 | Module | 14 | Expansion module interface; acts as MIDI CC track when no module inserted |
| 15 | Lights | 15 | DMX lighting control (up to 16 fixtures, via USB-C to USB-DMX adapter) |
| 16 | Motion | 16 | Visual sequencing via Photomatic (stop-motion) or Unity graphics (via app) |

---

## Synth Engines

12 synthesis-based sound engines total. Each engine has two unique parameters (P1/P2); all other parameters (filter, envelope, LFO, FX sends) are shared.

| Engine | ID | Type | P1 | P2 | Available On |
|--------|----|------|----|----|-------------|
| Saw | 26 | Filtered waves | Envelope | Tone | Bass, Lead, Arp, Chord |
| Uranus | 129 | Multi-oscillator electric ("clean bass") | Oscillator variation | Modulation | Bass |
| Electric | 98 | Complex/transforming synthesis | Cross mod | X mod | Bass, Lead, Arp, Chord |
| Digital | 25 | Digital raw | Octave | Feedback | Bass, Lead, Arp, Chord |
| Volt | 65 | Multi-oscillator electric | Oscillator variation | Modulation | Bass, Lead, Arp, Chord |
| Bow | 112 | String synthesis | Tension | Chorus | Bass, Lead, Arp, Chord |
| Cluster | 11 | Clustered detuned oscillators | Tone (detune) | Gravity (LP filter + noise) | Lead, Arp, Chord |
| Shade | 31 | Smooth piano | Detune | Drive | Chord |
| Organ | -- | 8 FM organ algorithms | Algorithm | Tweak | Synth tracks |
| EP | -- | 8 FM piano algorithms | Algorithm | Tone | Synth tracks |
| Sample | -- | PCM sample player | Crush (bitcrusher) | Cutoff | All synth tracks |

### Engine Assignments by Track (default order)
- **Bass**: Saw, Uranus, Electric, Digital, Volt, Bow
- **Lead**: Volt, Digital, Cluster, Saw, Electric, Bow
- **Arp**: Saw, Digital, Cluster, Volt, Electric, Bow(113?)
- **Chord**: Shade, Saw, Cluster, Digital, Volt, Bow

---

## MIDI CC Table (Complete)

### Parameter CCs (per-track, all 16 tracks)

Each parameter has an **absolute** CC and a **relative** CC. Absolute sets the value directly (0-127). Relative increments/decrements (64 = no change, >64 = increment, <64 = decrement; specifically 1 = decrement, 127 = increment).

All CC numbers below are **defaults** and can be reassigned per-track via `midi.json` or the OP-Z app.

| Parameter | Abs CC | Rel CC | Range | Notes |
|-----------|--------|--------|-------|-------|
| Parameter 1 (green encoder) | 1 | 32 | 0-127 | Engine-specific (see engine table) |
| Parameter 2 (blue encoder) | 2 | 33 | 0-127 | Engine-specific (see engine table) |
| Filter Cutoff | 3 | 34 | 0-127 | |
| Filter Resonance | 4 | 35 | 0-127 | |
| Envelope Attack | 5 | 36 | 0-127 | |
| Envelope Decay | 6 | 37 | 0-127 | |
| Envelope Sustain | 7 | 38 | 0-127 | |
| Envelope Release | 8 | 39 | 0-127 | |
| LFO Depth | 9 | 40 | 0-127 | |
| LFO Speed | 10 | 41 | 0-127 | |
| LFO Target | 11 | 42 | 0-127 | |
| LFO Shape | 12 | 43 | 0-127 | |
| FX 1 Send | 13 | 44 | 0-127 | |
| FX 2 Send | 14 | 45 | 0-127 | |
| Pan | 15 | 46 | 0-127 | 64 = center |
| Volume | 16 | 47 | 0-127 | |
| Portamento | 17 | 48 | 0-127 | |
| Note Style | 18 | 49 | 0-127 | |

### System CCs (global or per-track as noted)

| Parameter | CC | Rel CC | Range | Scope |
|-----------|----|--------|-------|-------|
| Track Gain | 50 | 51 | 0-127 | Per track (ch 1-16) |
| Reset Track Gains | 52 | -- | any | Any channel, any value |
| Mute | 53 | -- | 0-1 | Per track |
| Audio Mute | 54 | -- | 0-1 | Per track |
| Mute Group | 55 | -- | 0-9 | Any channel |
| Tempo | 56 | -- | 0-127 | Any channel |
| Swing | 57 | -- | 0-127 | Any channel |

### Track Parameter CCs

| Parameter | CC | Range | Scope |
|-----------|-----|-------|-------|
| Track Step Count | 60 | 1-16 | Per track |
| Track Step Length | 61 | 1-16 | Per track |
| Quantize | 62 | 0-127 | Per track |
| Note Length | 63 | 0-127 | Per track |

### UI / Navigation CCs

| Parameter | CC | Channel | Range | Notes |
|-----------|-----|---------|-------|-------|
| Active Track | 102 | 0 | 0-15 | Select which track is active |
| Parameter Page | 102 | 1 | 0-3 | Select encoder page |
| Select Pattern | 103 | -- | 0-15 | Pattern 1-16 |

### Other MIDI Messages

| Message | Notes |
|---------|-------|
| Note On/Off | Per-track on assigned channel |
| Pitch Bend | Per-track (ch 1-16) |
| Program Change | Pattern selection (bank 1-2, program 1-128 or bank 1-16/program 1-16 depending on `alt_program_change`) |
| MIDI Clock | Start/Stop/Continue, 24ppqn |

### Drum Tracks Note Mapping
Drum tracks (1-4) have 24 sounds mapped across the keyboard. The exact note mapping follows the OP-1 drum format convention.

---

## midi.json Format

### Location
Found in the `config` folder on the OP-Z's content mode disk. Access content mode by holding the track button while powering on.

### Complete Schema

```json
{
  "alt_program_change": true,
  "channel_one_to_active": false,
  "enable_program_change": true,
  "incoming_midi": true,
  "midi_echo": false,
  "outgoing_midi": true,
  "timing_clock_in": false,
  "timing_clock_out": true,
  "track_channels": [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
  "track_enable": [true, true, true, true, true, true, true, true, true, true, true, true, true, true, true, true],
  "parameter_cc_out": [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16],
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16],
    ...
  ]
}
```

### Field Reference

| Field | Type | Description |
|-------|------|-------------|
| `alt_program_change` | bool | `true`: bank 1-16 / program 1-16 for patterns. `false`: bank 1 / program 1-128 + bank 2 / program 1-32 |
| `channel_one_to_active` | bool | Route incoming MIDI on channel 1 to the currently active track |
| `incoming_midi` | bool | Enable incoming MIDI reception |
| `outgoing_midi` | bool | Enable outgoing MIDI transmission |
| `midi_echo` | bool | Echo incoming MIDI back on same port + MIDI through to other ports |
| `enable_program_change` | bool | Enable program change in/out |
| `timing_clock_in` | bool | Accept incoming MIDI clock |
| `timing_clock_out` | bool | Send MIDI clock |
| `track_channels` | int[16] | Outgoing MIDI channel per track (0-indexed, so 0 = ch 1) |
| `track_enable` | bool[16] | Enable/disable MIDI per track |
| `parameter_cc_out` | int[16][16] | CC number assignments per track (16 tracks x 16 parameters). Row = track index, column = parameter index matching CC 1-16 order (P1, P2, filter cutoff, filter res, ADSR x4, LFO x4, FX1, FX2, pan, volume) |

### parameter_cc_out Array Mapping
Each row is a track (0-15), each column maps to:

| Col | Parameter |
|-----|-----------|
| 0 | Parameter 1 |
| 1 | Parameter 2 |
| 2 | Filter Cutoff |
| 3 | Filter Resonance |
| 4 | Envelope Attack |
| 5 | Envelope Decay |
| 6 | Envelope Sustain |
| 7 | Envelope Release |
| 8 | LFO Depth |
| 9 | LFO Speed |
| 10 | LFO Target |
| 11 | LFO Shape |
| 12 | FX 1 Send |
| 13 | FX 2 Send |
| 14 | Pan |
| 15 | Volume |

### Example: Custom Mapping for External Synth
To map track 8 (chord) to control a Moog Sirin on channel 1:
```json
{
  "track_channels": [0, 1, 2, 3, 4, 5, 6, 0, 8, 9, 10, 11, 12, 13, 14, 15],
  "parameter_cc_out": [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16],
    ...
    [70, 71, 15, 16, 17, 18, 80, 80, 3, 13, 12, 85, 19, 21, 22, 66],
    ...
  ]
}
```

### Device Button MIDI Settings Access
Hold **tempo + screen** simultaneously, then press the corresponding key:

| Key | Setting |
|-----|---------|
| 1 | incoming_midi |
| 2 | outgoing_midi |
| 3 | timing_clock_in |
| 4 | timing_clock_out |
| 5 | enable_program_change |
| 6 | alt_program_change |
| 7 | channel_one_to_active |
| 8 | midi_echo |

---

## Step Components

Step components add unique playback behavior per step. Applied to audio tracks (1-8) only. Each step can have multiple components stacked.

### How to Apply
1. Hold **shift** and select target steps (LEDs turn green)
2. Keep holding shift, press a white piano key to select component type
3. Use value keys (1-0) to select the setting (1-10)

### Complete Component List

#### Rhythm Components

| Component | Key | Settings 1-10 | Description |
|-----------|-----|---------------|-------------|
| **Pulse** | C | Count: 1, 2, 3, 4, 5, 6, 7, 8, 9, Random | Replays the note for N additional steps |
| **Pulse Hold** | D | Count: 1, 2, 3, 4, 5, 6, 7, 8, 9, Random | Holds the note for N additional steps (sustain, no re-trigger) |
| **Multiply** | E | x1, x2, x3, x4, x5, x6, x7, x8, Broken Chord, Quantize | Replays note N times within one step. "Broken Chord" arpeggiates the chord on chord track |
| **Velocity** | F | -4, -3, -2, -1, Default, +1, +2, +3, Mute, Random | Adjusts step velocity relative to programmed value |

#### Pitch Components

| Component | Key | Settings 1-10 | Description |
|-----------|-----|---------------|-------------|
| **Ramp Up** | G | 2-step/1oct, 3/1, 4/1, 5/1, 6/1, 2-step/3oct, 3/3, 4/3, 5/3, 6/3 | Increases pitch over N steps within 1 or 3 octave range |
| **Ramp Down** | A | 2-step/1oct, 3/1, 4/1, 5/1, 6/1, 2-step/3oct, 3/3, 4/3, 5/3, 6/3 | Decreases pitch over N steps within 1 or 3 octave range |
| **Random** | B | 2-step/1oct, 3/1, 4/1, 5/1, 6/1, 2-step/3oct, 3/3, 4/3, 5/3, 6/3 | Randomizes pitch over N steps within 1 or 3 octave range |
| **Portamento** | C# | Glide 1-8, Direct, Random | Pitch glide from previous note to current. "Direct" = instant, "Random" = random glide time |

#### Modulation Components

| Component | Key | Settings 1-10 | Description |
|-----------|-----|---------------|-------------|
| **Sweep** | D# | Filter Up, Filter Down, Synth Up, Synth Down, Pan, Long Filter Up, Long Filter Down, Long Synth Up, Long Synth Down, Long Pan | Automates filter/synth/pan sweep from the step onward |
| **Tonality** | F# | Ignore progression, Transpose, Octave offset, Fifth offset, Third offset, Chromatic up, Chromatic down, Quantize 1, Quantize 2, Quantize 3 | Controls how notes interact with master track transpose. "Ignore progression" = static pitch |

#### Sequencer Flow Components

| Component | Key | Settings 1-10 | Description |
|-----------|-----|---------------|-------------|
| **Jump** | G# | Start, 2/4, 3/4, 4/4, Forward, Back, Random, Stay, --, -- | Controls playhead position when this step is reached |
| **Gate Step** | A# | Aligns step trigger to global track timing | Controls whether step triggers based on another track's timing |

#### Spark Components (Conditional Triggers)

| Component | Key | Settings 1-10 | Description |
|-----------|-----|---------------|-------------|
| **Parameter Spark** | -- | 1, 1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 1-8, Random, Reset counter | Parameter lock fires every Nth cycle. "1-3" means fire on cycles 1, 2, 3 then skip |
| **Component Spark** | -- | 1, 1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 1-8, Random, Reset counter | Step component fires every Nth cycle |
| **Trigger Spark** | -- | 1, 1-2, 1-3, 1-4, 1-5, 1-6, 1-7, 1-8, Random, Reset counter | Step itself triggers every Nth cycle. Creates evolving patterns |

---

## Physical Controls

### Layout Overview

The OP-Z is a flat 10mm-thick slab with all controls on the top surface, one button on the front edge, and a pairing button on the back.

### Encoders (4, top-left area)

Color-coded endless rotary dials (flush-mount, spun with fingertip). Each controls a different parameter depending on the active page.

| Encoder | Color | Page 1 (Green) | Page 2 (Blue) | Page 3 (Yellow) | Page 4 (Red) |
|---------|-------|----------------|---------------|-----------------|--------------|
| 1 | Green | Parameter 1 | Envelope Attack | LFO Depth | FX 1 Send |
| 2 | Blue | Parameter 2 | Envelope Decay | LFO Speed | FX 2 Send |
| 3 | Yellow | Filter Cutoff | Envelope Sustain | LFO Target | Pan |
| 4 | Red | Filter Resonance | Envelope Release | LFO Shape | Volume |

**Notes:**
- For drum tracks: yellow = filter cutoff, red = resonance (always)
- Page is selected by pressing **shift** + encoder or cycling with shift
- Each encoder has an adjacent RGB LED showing value/page

### Index Buttons (4, top row behind encoders)

| Button | Primary | Hold | Triple-click |
|--------|---------|------|-------------|
| **Project** | Select project (1-10) | Pattern/chain view | Lock to project screen |
| **Mixer** | Track mute/unmute | Group gain, master compressor | Lock to mixer screen |
| **Tempo** | Tap tempo | Adjust BPM (with green dial), swing | Lock to tempo screen |
| **Screen** | App screen navigation | Battery level check | Lock to screen view |

### Track Buttons (16, main top row)

Multifunction depending on context:
- **Default**: Select track (tap corresponding button)
- **While sequencer plays**: Step triggers (toggle steps on/off)
- **Hold Project + press**: Select pattern (1-16)
- **Hold Track + press**: Access track settings

### Transport Buttons (bottom area)

| Button | Function | Combo |
|--------|----------|-------|
| **Play** | Start/continue playback | |
| **Stop** | Stop playback | Double-tap = super panic (clear audio buffers) |
| **Record** | Toggle recording mode | Hold + play = live record; Hold + step = step record |

### Musical Keyboard (bottom row, 16 keys)

- **White keys**: Play notes / select step components
- **Black keys**: Set values (1-10) for step components
- **Octave +/-**: Transpose keyboard up/down

### Special Controls

| Control | Location | Function |
|---------|----------|----------|
| **Shift** | Left side | Toggle parameter pages; enable punch-in effects; hold for step component mode |
| **Pitch Bend** | Front edge (rubber pad) | Pressure-sensitive, bidirectional. Pitch bend on audio/tape/master tracks. Also adjusts step velocity in sequencer |
| **Microphone** | Built-in | Sampling input |
| **Speaker** | Built-in | Audio output when no headphones/line connected |
| **Pairing Button** | Back | Bluetooth LE pairing for app connection |

### Shift Combos

| Combo | Function |
|-------|----------|
| Shift + encoder | Cycle parameter page |
| Shift + step buttons | Enter step component mode (select steps to modify) |
| Shift + white keys | Select component type |
| Shift + black keys | Set component value |
| Track + Stop | Clear track |
| Tempo + Screen (hold both) | Access MIDI settings (then press keys 1-8 to toggle) |
| Project + track button | Select pattern |

### Ports

| Port | Location | Function |
|------|----------|----------|
| USB-C | Left side | MIDI, audio, charging, content mode |
| 3.5mm jack | Left side | Stereo line out |

---

## SysEx Protocol

Teenage Engineering manufacturer ID: `00 20 76`, Device ID: `01`

### Message Format
```
F0 00 20 76 01 [command] [data...] F7
```

### 7-bit Encoding
All SysEx data uses 7-bit encoding. Data packed in 8-byte chunks: first byte holds MSBs of following 7 bytes. Some messages use zlib compression (signature `78 9C`, appears as `78 1C` after encoding).

### Key SysEx Messages

| Msg | Command | Description |
|-----|---------|-------------|
| $00 | Master Heartbeat | Must be sent regularly to maintain connection |
| $03 | Keyboard Setting | Octave, track selection |
| $06 | Button States | Encoder mode, button holds |
| $0E | Sound Preset | All synth parameters (27 bytes: engine, ADSR, FX, filter, pan, level, portamento, LFO/arp) |
| $35 | File Request | Retrieve settings/projects |
| $52 | File Client Heartbeat | File transfer keep-alive |
| $53 | File Data Transfer | Project/preset data |
| $62 | Text Commands | Debug mode, controller mode |

---

## Content Mode (Disk Mode)

### Entering Content Mode
Hold **track** button while powering on. All track LEDs turn green.

### Folder Structure
```
OP-Z/
  config/
    midi.json          # MIDI configuration
    how_to_import.txt  # Sample import instructions
    how_to_dmx.txt     # DMX configuration instructions
  samplepacks/
    1/                 # Kick track sample slots (1-10)
    2/                 # Snare track sample slots (1-10)
    3/                 # Perc track sample slots (1-10)
    4/                 # Sample track sample slots (1-10)
    5/                 # Bass track sample slots (1-10)
    6/                 # Lead track sample slots (1-10)
    7/                 # Arp track sample slots (1-10)
    8/                 # Chord track sample slots (1-10)
  projects/            # Project files
  bounces/             # Audio bounces (export only)
  rejected/            # Appears when import fails
```

### File Operations

| Content | Add | Modify | Remove |
|---------|-----|--------|--------|
| Projects | Yes | Yes | Yes |
| Sample packs | Yes | Yes | Yes |
| Bounces | No | No | Yes |
| Config (midi.json etc.) | No | Yes | No |

### Sample Format
- OP-1 format `.aif` files
- Drum tracks: drum sample format
- Synth tracks: synth sample format
- Maximum capacity: 24MB total sample data

**Important**: Changes only take effect after safely ejecting the USB disk. The unit updates and restarts in normal mode.

---

## Sync
- USB MIDI clock (send/receive, 24ppqn)
- Can sync with other TE devices via USB-C daisy chain
- Bluetooth MIDI clock (less reliable than USB)

## Limitations
- No built-in screen -- requires app for detailed editing
- Step component effects are internal only (not directly MIDI-controllable)
- 16 steps per pattern (expandable with pattern chaining up to 10x = 160 steps)
- Parameter locks are stored in sequence, not transmitted as CC during playback unless configured
- No DIN MIDI (USB-C and BLE only)
- 24MB sample memory limit

---

## 3rd-Party GitHub Repos

### MIDI Parsers & Protocol
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [nbw/opz](https://github.com/nbw/opz) | JS | 62 | Web MIDI parser for OP-Z. Decodes track, action (keys/dial/pitch bend), velocity, note. Includes MIDI Tetris example |
| [ayamflow/opz-parser](https://github.com/ayamflow/opz-parser) | JS | -- | WIP MIDI parser, returns track/key/action objects |
| [pencilresearch/midi](https://github.com/pencilresearch/midi/blob/main/Teenage%20Engineering/OP-Z.csv) | CSV | -- | MIDI CC & NRPN database with OP-Z mappings |
| [hyphz/opzdoc](https://github.com/hyphz/opzdoc/wiki/MIDI-Protocol) | Wiki | -- | Reverse-engineered SysEx protocol, 7-bit encoding, message types |
| [MarkRdgOx/opzdoc](https://github.com/MarkRdgOx/opzdoc/wiki/MIDI-Protocol) | Wiki | -- | Fork of opzdoc with MIDI protocol documentation |

### MIDI Configuration & Control
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [nickbec10/MIDI_Config_OP-Z_Moog_Sirin](https://github.com/nickbec10/MIDI_Config_OP-Z_Moog_Sirin) | JSON | 2 | Example midi.json mapping OP-Z to Moog Sirin |
| [tsoop-com/op-z-m-vave-smk-25](https://github.com/tsoop-com/op-z-m-vave-smk-25) | -- | 15 | MIDI bindings for wireless M-Vave SMK-25 controller |
| [shalashify/pytribe](https://github.com/shalashify/pytribe) | Python | 9 | Use OP-Z as MIDI sequencer to trigger computer samples |

### Backup & Content Tools
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [chrisdiana/OPZgo](https://github.com/chrisdiana/OPZgo) | Python | 37 | Ultra-portable backups (designed for Raspberry Pi) |
| [brianmichel/Operator](https://github.com/brianmichel/Operator) | Swift | 6 | Backup, restore, load samples (OP-1 + OP-Z) |
| [romangarms/OP-1Z-Sample-Manager](https://github.com/romangarms/OP-1Z-Sample-Manager) | -- | 10 | Desktop app for sample management (OP-Z + OP-1) |
| [schollz/teoperator](https://github.com/schollz/teoperator) | Go | 162 | Make OP-1/OP-Z drum and synth patches from any sound |
| [AlexCharlton/op-patch-util](https://github.com/AlexCharlton/op-patch-util) | -- | 33 | CLI tool for creating/modifying OP-1 and OP-Z patches |

### File Format & Research
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [lrk/z-po-project](https://github.com/lrk/z-po-project) | -- | 79 | Extensive OP-Z research: file format reverse-engineering, engine docs, sample packs |

### Visual / DMX
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [keijiro/VideolabTest](https://github.com/keijiro/VideolabTest) | Unity | 209 | OP-Z Videolab examples |
| [Videolab-Creators-Group/Tape-Track-Videopak](https://github.com/Videolab-Creators-Group/Tape-Track-Videopak) | Unity | 14 | Tape track effects videopak |
| [Videolab-Creators-Group/Videolab-Resources](https://github.com/Videolab-Creators-Group/Videolab-Resources) | -- | 9 | Resources for making videopaks |
| [Videolab-Creators-Group/Videopak-Template](https://github.com/Videolab-Creators-Group/Videopak-Template) | Unity | 1 | Unity project template for videopak development |
| [romangarms/Romans-VideoPaks](https://github.com/romangarms/Romans-VideoPaks) | Unity | 9 | Videopak collection (Tombola, XYpak) |
| [mochreach/chords](https://github.com/mochreach/chords) | C# | 38 | Chords UI videopak for OP-Z |

### Misc
| Repo | Lang | Stars | Description |
|------|------|-------|-------------|
| [xmacex/connect-opz](https://github.com/xmacex/connect-opz) | -- | 36 | Connect OP-Z as audio device on norns |
| [IvanWoo/opz-key-finder](https://github.com/IvanWoo/opz-key-finder) | -- | 4 | Key reference while jamming |
| [KingDuane/OP-Z-Teensy-LEGO-Module](https://github.com/KingDuane/OP-Z-Teensy-LEGO-Module) | C++ | 0 | Teensy-based LEGO module for OP-Z expansion port |
