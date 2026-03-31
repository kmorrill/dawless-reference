# Expressive E Osmose

## Overview
- **Type**: MPE polyphonic synthesizer + expressive MIDI controller
- **Keys**: 49 (original) / 61 (newer model), proprietary "Augmented Keyboard Action" (A.K.A.)
- **Polyphony**: 24 voices (often 12 stereo voices)
- **Engine**: Haken EaganMatrix (6 SHARC DSPs) — same as Continuum Fingerboard
- **Expression**: Per-note 3-axis expression (strike, pressure/slide, lateral pitch bend)
- **Presets**: 580 factory, up to 2,048 slots
- **Power**: External PSU 12V 1.5A (not USB-powered)
- **Dimensions (49-key)**: 895 x 310 x 90mm, 8.3kg

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| USB-B (Port 1 "USB Play") | In/Out | MIDI controller output to DAW/external gear |
| USB-B (Port 2 "USB Haken") | In/Out | EaganMatrix engine input; Haken Editor communication |
| MIDI DIN In | In | 5-pin DIN |
| MIDI DIN Out/Thru | Out | 5-pin DIN, configurable (keyboard out, DSP input, thru, or CVC) |
| Audio Out | Out | 2x 1/4" (L/R) |
| Headphone | Out | 1/4" stereo |
| Pedal 1 | In | 1/4" TRS, configurable (sustain or expression) |
| Pedal 2 | In | 1/4" TRS, configurable (sustain or expression) |

**Note**: USB presents as two virtual MIDI devices on the host. Avoid recording channels 15-16 from USB Haken port (Haken Editor communication data).

## Keybed Expression Axes
| Axis | Gesture | Range |
|------|---------|-------|
| Z (vertical) | Key press depth | ~9mm initial dip + extended aftertouch travel |
| X (lateral) | Key wiggle left/right | Blacks ±4mm, whites nearly as far when depressed |
| "Y" (emulated) | Deeper key press beyond initial travel | Derived from pressure depth, not true front-to-back slide |

**Important**: The Osmose has no true Y-axis (front-to-back) sensing. The "slide" dimension (CC 74) is derived from deeper key pressure, so pressure and slide are somewhat coupled.

## MPE Implementation

### Expression Dimensions
| Dimension | MIDI Message | Source Gesture |
|-----------|-------------|----------------|
| Strike | Note-on velocity | Initial key press speed |
| Glide (X) | Per-channel pitch bend | Lateral key wiggle (left/right) |
| Pressure | Per-channel channel pressure (aftertouch) | Key press depth (first range) |
| Slide/Timbre | Per-channel CC 74 | Key press depth (extended/deeper range) |
| Lift | Note-off velocity | Release speed |

### MPE Zone Setup
- **Lower Zone**: Channel 1 = global, Channels 2-16 = member channels (up to 15 voices)
- Member channel count is reducible via settings
- Pitch bend range: configurable (typically ±48 semitones; physical key travel is small, so bending fraction must be tuned to avoid overshoot)

### MIDI Output Profiles (4 modes)
| Profile | Channel(s) | Velocity | Aftertouch | Pitch Bend | Best For |
|---------|-----------|----------|------------|------------|----------|
| **MPE** (default) | Ch 1 global, Ch 2-16 per-note | Yes | Channel pressure per-channel + CC 74 | Per-channel | MPE-compatible synths/plugins |
| **Classic Keyboard** | Ch 1 only | Yes | Mono channel pressure | Disabled | Legacy MIDI gear |
| **Poly Aftertouch** | Ch 1 only | Yes | Polyphonic aftertouch | Disabled | Synths with poly AT (e.g., OB-6) |
| **Multi-Channel** | Ch 1-16 per-note, no global | Yes | Per-channel | Per-channel | Non-MPE gear that benefits from multi-channel |

In Multi-Channel mode, global messages (e.g., CC 1 from mod slider) are sent as copies on every active channel.

### MPE+
Proprietary Haken protocol extending MPE with higher time and magnitude resolution. Replaces traditional velocity with "attack trajectory" analysis. Available on USB Haken port only.

## Physical Front Panel Controls

### Layout (left of keybed, top to bottom)

| Control | Type | Position | Function |
|---------|------|----------|----------|
| **Mode Button (M)** | Backlit button | Top-left, above screen | Toggles Sound Engine mode / External MIDI mode |
| **Menu Buttons (1-4)** | 4 backlit buttons | Top row, right of Mode button | Each opens one of the 4 menus for the current mode (context-dependent labels on screen) |
| **LCD Screen** | Color display | Center of control panel | Shows menus, parameters, preset names, waveform/matrix views |
| **Tab Selector** | Clickable encoder | Left of screen, mid-row | Scrolls through tabs within a menu; click freezes Sensitivity/Playing settings across preset changes |
| **Parameter Selector** | Clickable encoder | Right of screen, mid-row | Scrolls parameters within a tab; click starts/pauses Arpeggiator or toggles Pressure Glide |
| **Value Encoders 1-4** | 4 clickable encoders | Row below screen | Adjust parameter values shown above each encoder; click for context actions |
| **Pitch Slider** | Spring-return slider | Far left, below encoders | Pitch bend on global channel (Ch 1 in MPE); spring-loaded to center |
| **Modulation Slider** | Non-spring slider | Next to pitch slider | CC 1 on global channel; stays where positioned |
| **Preset Up/Down** | 2 buttons | Next to sliders | Navigate through preset list (up/down) |
| **Octave Up/Down** | 2 buttons | Next to preset buttons | Transpose keybed +/- 1-2 octaves |
| **Volume Knob** | Small knob | Front edge of case, right of headphone jack | Master/headphone volume (not MIDI-transmitting) |

**Note**: All buttons and encoders surrounding the screen are "soft" controls -- their functions change based on the current screen context. There are no fixed-function knobs for synthesis parameters; deep editing requires the Haken Editor software.

### MIDI Output from Physical Controls

| Control | MIDI Message | Port | Channel |
|---------|-------------|------|---------|
| Pitch slider | Pitch Bend | USB Play (Port 1) / DIN | Global (Ch 1 in MPE) |
| Modulation slider | CC 1 (Mod Wheel) | USB Play (Port 1) / DIN | Global (Ch 1 in MPE) |
| Pedal 1 | Configurable, default CC 64 (Sustain) | USB Play (Port 1) / DIN | Global (Ch 1 in MPE) |
| Pedal 2 | Configurable, typically expression (continuous) | USB Play (Port 1) / DIN | Global (Ch 1 in MPE) |

In Multi-Channel mode, global messages (CC 1, pedals) are sent as copies on every active channel.

## Program Change and Preset Selection via MIDI

*Added in firmware 2.0 (September 2024).*

### Protocol

Preset selection uses standard **Bank Select (CC#0) + Program Change** on **MIDI Channel 1**, received on:
- **USB MIDI Port 2** (Haken Port) -- always active
- **DIN MIDI In** -- only when DIN mode is set to "dsp in", "dsp in+thru", or "cvc"

**Not accepted on USB Port 1 (Play port).**

### Bank/Program Mapping (Playlist System)

| MIDI Message | Value Range | Selects |
|-------------|-------------|---------|
| CC#0 (Bank Select MSB) | 0-29 | Playlist 1-30 |
| Program Change | 0-127 | Preset slot 1-128 within selected playlist |

- 30 user playlists, 128 slots each (3,840 addressable presets)
- Up to 2,048 onboard presets total
- Bank Select must precede Program Change (standard MIDI convention)
- Display numbering: Osmose shows 1-128 (matching Ableton Live convention); wire values are 0-127

### Viewing MIDI Address of a Preset

The preset info popup on the Osmose screen displays the Bank Select (CC#0) and Program Change values needed to recall that specific preset remotely.

### Favorites

Firmware 2.0 also added a Favorites list, but it is not separately addressable via Bank Select -- favorites are organized into playlists for MIDI recall.

## MIDI Clock and Sync

### Clock Reception

The Osmose **receives** MIDI clock for arpeggiator synchronization. It does **NOT send** MIDI clock.

> **"Attention: Osmose is not capable of sending its own internal clock."** -- Official documentation

### Clock Source Selection

The `ext clock source` setting in the MIDI I/O tab selects which port provides clock:
- USB Play (Port 1) -- default
- USB Haken (Port 2)
- DIN MIDI In

Toggle between internal tempo and external clock sync by pressing the Value Encoder below the tempo display.

### Transport

- **Start/Stop**: No documented support for MIDI Start (0xFA) / Stop (0xFC) / Continue (0xFB) transport messages
- The arpeggiator is started/paused manually via the Parameter Selector knob (press to toggle)
- No sequencer or song position pointer support

### Arpeggiator Timing

- Internal BPM is adjustable via the Playing menu
- When synced to external clock, arpeggiator follows incoming MIDI clock tempo
- The arpeggiator is expressive/MPE-aware with per-note glide control

### Summary

| Feature | Supported? |
|---------|-----------|
| Receive MIDI Clock | Yes (arp sync) |
| Send MIDI Clock | **No** |
| Receive Transport Start/Stop | Not documented |
| Send Transport Start/Stop | No |
| Song Position Pointer | No |

## Haken Editor / USB Port 2 Protocol

### Port 2 Overview

USB Port 2 ("Haken Port") is the communication channel between the Osmose's EaganMatrix engine and the Haken Editor software. It presents as a separate USB MIDI device:
- **macOS**: "Osmose Port 2" (or localized equivalent)
- **Windows**: "MIDIIN2/MIDIOUT2 (Osmose)"

### What Flows Over Port 2

| Direction | Data | Purpose |
|-----------|------|---------|
| Host -> Osmose | CC messages on Ch 1 | Macro/parameter control (see CC map below) |
| Host -> Osmose | CC messages on Ch 16 | Haken Editor protocol (preset load, engine config) |
| Host -> Osmose | Bank Select (CC#0) + Program Change on Ch 1 | Preset selection |
| Host -> Osmose | MPE note/expression data on Ch 1-14 | Trigger internal synth from external controller |
| Osmose -> Host | CC messages on Ch 15-16 | Haken Editor state feedback |
| Osmose -> Host | MPE+ data | High-resolution expression (proprietary Haken protocol) |
| Bidirectional | CC-based protocol on Ch 15-16 | Haken Editor bidirectional sync |

### Channels 15-16 Warning

MIDI channels 15 and 16 are reserved for Haken Editor communication. **Always filter these channels when recording from Port 2** to avoid capturing editor protocol data that will sound wrong on playback.

### Protocol Format

The Haken protocol uses **CC messages** (not SysEx) for editor communication. Preset files in the Haken Editor folder structure are stored as `.mid` files containing CC sequences. Example: CC#55 value 0 on channel 16 resets preset loading state.

**The protocol is proprietary and not publicly documented by Haken Audio.** The copyright notice reads: "Haken Audio Midi Protocol Copyright (C) by Lippold Haken."

### Bidirectional Sync Limitations

> "There is no full bidirectional synchronization between Osmose and Haken Editor. It is expected that the user interface of Osmose will not update when changing macros via the Haken Editor."

### MPE+ (Port 2 Only)

MPE+ is a proprietary Haken extension of MPE with higher time and magnitude resolution. It replaces traditional velocity with "attack trajectory" analysis. Only available on USB Port 2.

### USB Haken Output Modes

Configurable via `usb haken mode` in MIDI I/O settings:
| Mode | Output |
|------|--------|
| Disabled (default) | No output on Port 2 |
| Note only (MPE+) | MPE+ performance data |
| Note & matrix (MPE+) | MPE+ performance data + editor communication |

### EaganMatrix CC Map (received on Port 2, Channel 1)

| Parameter | CC # | Range |
|-----------|------|-------|
| Macro i | CC 12 | 0-127 (14-bit capable) |
| Macro ii | CC 13 | 0-127 (14-bit capable) |
| Macro iii | CC 14 | 0-127 (14-bit capable) |
| Macro iv | CC 15 | 0-127 (14-bit capable) |
| Macro v | CC 16 | 0-127 (14-bit capable) |
| Macro vi | CC 17 | 0-127 (14-bit capable) |
| Postgain (output volume) | CC 18 | 0-127 |
| Global FX Parameter 1 | CC 20 | 0-127 |
| Global FX Parameter 2 | CC 21 | 0-127 |
| Global FX Parameter 3 | CC 22 | 0-127 |
| Global FX Parameter 4 | CC 23 | 0-127 |
| Global FX Mix | CC 24 | 0-127 |
| Pregain (input gain) | CC 26 | 0-127 |
| Sustain | CC 64 | 0/127 (switch) |
| Sostenuto 1 | CC 66 | 0/127 (switch) |
| Sostenuto 2 | CC 69 | 0/127 (switch) |
| EQ Tilt Value | CC 83 | 0-127 |
| EQ Frequency | CC 84 | 0-127 |
| EQ Mix | CC 85 | 0-127 |
| Compressor Mix | CC 93 | 0-127 |

Macros i-vi and Volume support 14-bit MIDI (CC 12-17 MSB, CC 44-49 LSB). Recirculator parameters are 7-bit only.

## 3rd-Party GitHub Repositories

### Directly Relevant

| Repository | Author | Description |
|-----------|--------|-------------|
| [pachde-CHEM](https://github.com/Paul-Dempsey/pachde-CHEM) | Paul Dempsey | **Controller for Haken EaganMatrix** -- VCV Rack plugin. Full preset browser, macro control, and matrix editing for Osmose/Continuum/ContinuuMini. Uses the proprietary Haken Audio MIDI Protocol. Most comprehensive open-source EaganMatrix integration. |
| [pachde-hc-one](https://github.com/Paul-Dempsey/pachde-hc-one) | Paul Dempsey | **Predecessor to CHEM** -- VCV Rack modules (HC-1, HC-2, Pedals, Favorites, Round, Compress, TiltEQ). Performance companion for EaganMatrix devices. Supports firmware 10.40+. |
| [osmose-usersettings](https://github.com/segfault16/osmose-usersettings) | segfault16 | **Preset management tool** (Python). Dumps user presets via Osmose Updater, provides CLI for reordering/renaming presets. Works with the `.mid` preset files and group configuration files. MIT license. |

### Tangentially Relevant

| Repository | Description |
|-----------|-------------|
| [moDllz](https://github.com/dllmusic/moDllz) | VCV Rack modules with MPE support; has documented Haken MPE+ mode issues (Issue #27) |
| [MPE (alexandreleroux)](https://github.com/alexandreleroux/MPE) | MPE MIDI plugins for VCV Rack |
| [PatchStorage EaganMatrix](https://patchstorage.com/platform/eagan-matrix/) | Not GitHub, but the main community preset sharing platform for EaganMatrix patches |

### Notable Absence

There is no official open-source SDK, protocol documentation, or reference implementation from either Expressive E or Haken Audio. The pachde projects by Paul Dempsey represent the most complete reverse-engineering effort, developed with cooperation from Haken Audio ("generous support by sharing internal technical details").

## Internal Synth (EaganMatrix)
- Haken Audio's EaganMatrix — deep modular synthesis engine on 6 SHARC DSPs
- Synthesis types: virtual analog, FM, physical modeling, granular, additive, convolution
- Architecture: 5 oscillators/waveshapers, noise sources, 5 multi-mode filters, 2 filter banks (8 or 48 filters), time delays, convolution module, recirculator
- Built-in effects: reverb, delay/echo, compressor/drive, shelving EQ
- All parameters respond natively to MPE expression data
- Preset editing via Haken Editor software (Mac/Win), not from front panel

## Limitations
- **No true Y-axis**: Slide (CC 74) derived from pressure depth, not finger position — pressure and slide are coupled
- **Monotimbral only**: Cannot split zones to two different instruments simultaneously
- **Pitch bend range mismatch**: External MPE synths often need careful tuning of bending fraction to match Osmose's small physical key travel
- **DIN MIDI bandwidth**: Standard DIN may bottleneck with heavy MPE data from 49 expressive keys — USB strongly preferred
- **Not USB-powered**: Requires external PSU
- **24-voice ceiling**: Voice stealing may occur in dense passages (especially at 12 stereo voices)
- **Keybed feel**: Springy, laterally-mobile action designed for expression — unfamiliar to traditional pianists
- **No front-panel preset editing**: Requires Haken Editor software for deep sound design
