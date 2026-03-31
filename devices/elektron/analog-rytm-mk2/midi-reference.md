# Analog Rytm MKII - MIDI Reference

This document contains all documented MIDI control capabilities for the Elektron Analog Rytm MKII drum machine, extracted from the official user manual.

## Overview

The Analog Rytm MKII can be controlled via:
- **MIDI DIN** (5-pin MIDI cables via MIDI IN/OUT/THRU ports)
- **USB MIDI** (class compliant, no drivers required)
- **DIN Sync 24/48** (legacy sync protocol for vintage gear)

## MIDI Note Control

### Track Triggering (Notes 0-11)
**MIDI Notes C0-B0** (sometimes labeled C-2 to B-2)

| MIDI Note | Note Name | Track | Sound |
|-----------|-----------|-------|-------|
| 0 | C0 | 1 | Bass Drum (BD) |
| 1 | C#0 | 2 | Snare Drum (SD) |
| 2 | D0 | 3 | Rim Shot (RS) |
| 3 | D#0 | 4 | Hand Clap (CP) |
| 4 | E0 | 5 | Bass Tom (BT) |
| 5 | F0 | 6 | Low Tom (LT) |
| 6 | F#0 | 7 | Mid Tom (MT) |
| 7 | G0 | 8 | Hi Tom (HT) |
| 8 | G#0 | 9 | Closed Hihat (CH) |
| 9 | A0 | 10 | Open Hihat (OH) |
| 10 | A#0 | 11 | Cymbal (CY) |
| 11 | B0 | 12 | Cowbell (CB) |

**Notes:**
- These note values trigger the corresponding track regardless of which track is active
- Each track must be set to its MIDI channel (default channels 1-12)
- Tracks 3/4, 7/8, 9/10, 11/12 share voices - right track has priority if both triggered

### Chromatic Playback (Notes 12-59)
**MIDI Notes C1-B4** (4 octaves)

- Notes 12-59 trigger the **active track** chromatically
- Spans 48 chromatic variations (4 octaves)
- The track Sound must have **chromatic mode enabled** in SOUND SETTINGS
- Only works for the currently selected/active track

**Example:**
- MIDI note 12 (C1) = active track at lowest pitch
- MIDI note 24 (C2) = active track +12 semitones
- MIDI note 59 (B4) = active track at highest pitch

## MIDI Program Changes

### Pattern Selection
**Program Change Messages 0-127** select patterns A01-H16

| Program Change | Pattern |
|----------------|---------|
| 0 | A01 |
| 1 | A02 |
| ... | ... |
| 15 | A16 |
| 16 | B01 |
| ... | ... |
| 127 | H16 |

**Configuration:**
- Enable/disable: `GLOBAL SETTINGS > MIDI CONFIG > SYNC > PRG CH RECEIVE`
- Set input channel: `GLOBAL SETTINGS > MIDI CONFIG > CHANNELS > PROGRAM CH IN CH`
- Can use AUTO channel or specific channel

## MIDI Channels

### Channel Assignment
Configure in: `GLOBAL SETTINGS > MIDI CONFIG > CHANNELS`

| Setting | Description |
|---------|-------------|
| **TRACK 1-12** | Individual MIDI channel per drum track (OFF to disable) |
| **FX** | MIDI channel for FX track |
| **PERF CHANNEL** | Channel for Performance mode knob data |
| **AUTO CHANNEL** | Controls currently active track (useful for keyboard control) |
| **PROGRAM CH IN CH** | Listens for program changes (can use AUTO) |
| **PROGRAM CH OUT CH** | Sends program changes when patterns change |

**Notes:**
- Setting a track to OFF makes it ignore all MIDI messages
- AUTO CHANNEL is useful when switching between tracks with an external keyboard
- Each track can be on a different channel

## MIDI Clock & Sync

### Clock Settings
Configure in: `GLOBAL SETTINGS > MIDI CONFIG > SYNC`

| Setting | Description |
|---------|-------------|
| **CLOCK RECEIVE** | Respond to incoming MIDI clock from external devices |
| **CLOCK SEND** | Transmit MIDI clock |
| **TRANSPORT RECEIVE** | Respond to start/stop/continue/song position messages |
| **TRANSPORT SEND** | Send start/stop/continue/song position messages |
| **PRG CH RECEIVE** | Respond to incoming program changes |
| **PRG CH SEND** | Send program changes when patterns change |

**Important Sync Behavior:**
- If both CLOCK RECEIVE and TRANSPORT RECEIVE are active, the device waits 16 seconds for clock after start/continue before considering sync lost
- This allows for DAW preroll functionality
- If only TRANSPORT RECEIVE is enabled, starts immediately upon receiving start/continue

### Stop Behavior
- Quick double-press `[STOP] + [STOP]` stops playback and fades out effects
- This also sends **All Sound Off** message over MIDI

## MIDI CC & NRPN Control

### Parameter Control
**Configuration:** `GLOBAL SETTINGS > MIDI CONFIG > PORT CONFIG`

**PARAM OUTPUT** setting controls what the DATA ENTRY knobs send:
- **NRPN**: Send NRPN MIDI messages
- **CC**: Send CC MIDI messages

**Note:** The manual references "APPENDIX C: MIDI" for complete CC/NRPN parameter mappings, but this appendix is not included in the available documentation. The specific CC and NRPN numbers for each parameter are not documented in the main manual.

### Control Destinations
Configure what sends MIDI data:

| Setting | Options | Description |
|---------|---------|-------------|
| **ENCODER DEST** | INT / INT+EXT | DATA ENTRY and TRACK LEVEL knobs |
| **PAD DEST** | INT / INT+EXT / EXT | Pad triggers |
| **PRESSURE DEST** | INT / INT+EXT / EXT | Pad pressure/aftertouch |
| **MUTE DEST** | INT / INT+EXT / EXT | Track mutes |

- **INT**: Internal only, no MIDI sent
- **INT+EXT**: Send internally and via MIDI
- **EXT**: External MIDI only

### Receiving Control Data
**RECEIVE NOTES**: Enable to play sounds via external MIDI keyboard
**RECEIVE CC/NRPN**: Enable to control parameters from external MIDI devices

## MIDI Port Configuration

### Port Settings
Configure in: `GLOBAL SETTINGS > MIDI CONFIG > PORT CONFIG`

#### Input/Output Routing
| Setting | Options | Description |
|---------|---------|-------------|
| **INPUT FROM** | DISABLED / MIDI / USB / MIDI+USB | Where device receives MIDI |
| **OUTPUT TO** | DISABLED / MIDI / USB / MIDI+USB | Where device sends MIDI |
| **OUTPUT CH** | AUTO / TRACK | Pads/knobs send on auto or track channel |

**Note:** MIDI+USB mode can limit USB transfer speeds when sending large data

#### MIDI OUT Port Function
**OUT PORT FUNCTIONALITY**: Select signal type for MIDI OUT port
- **MIDI**: Standard MIDI data (default)
- **DIN 24**: DIN 24 sync pulses (no MIDI data sent)
- **DIN 48**: DIN 48 sync pulses (no MIDI data sent)

#### MIDI THRU Port Function
**THRU PORT FUNCTIONALITY**: Same options as OUT PORT
- Forwards data from MIDI IN by default
- Can also send DIN sync signals

### Turbo Speed
**TURBO SPEED**: Negotiates higher MIDI transfer speed
- Requires compatible Turbo-MIDI interface
- Press `[YES]` to start negotiation (speed chosen automatically)

## Sequencer MIDI Output

### Per-Track MIDI Send
Configure in: `TRACK MENU > TRIGS SETUP > TRK SEND MIDI`

When enabled for a track:
- Sends **note on/off** (from NOT and LEN parameters)
- Sends **velocity** (from VEL parameter)
- Sends on the track's assigned MIDI channel
- Data is sent **both externally and to internal sound**
- Only sequencer note/velocity data is sent (not parameter locks)

**Use Case:** Control external MIDI gear from Analog Rytm sequencer

## SysEx

### SysEx Dump (Backup/Transfer)
Access: `GLOBAL SETTINGS > SYSEX DUMP`

#### SysEx Send
Send data to external device via MIDI OUT or USB:
- **WHOLE PROJECT**: All kits, sounds, patterns, songs, globals
- **ALL SOUNDS**: All sounds (no samples)
- **ALL KITS**: All 128 kits
- **ALL PATTERNS**: All 128 patterns
- **ALL SONGS**: All 16 songs
- **ALL GLOBALS**: All 4 global slots
- **ALL SETTINGS**: All settings
- Individual kit, sound, pattern, song, or global

**Note:** Samples cannot be transferred via SysEx (use Elektron Transfer software)

#### SysEx Receive
Receive data from external device via MIDI IN or USB:
- **ANYTHING**: Receive any viable SysEx data
- **KIT**: Active kit or specific kit slot (1-128)
- **SOUND POOL**: To project sound pool
- **+DRIVE SOUND**: To +Drive sound library
- **PATTERN**: Specific pattern slot
- **SONG**: Specific song slot
- **GLOBAL**: Specific global slot

**Important:**
- Device can receive SysEx at any time, regardless of active menu
- Must connect MIDI or USB before initiating transfer
- Recommended tool: Elektron C6 utility (free download)

## USB Configuration

### USB Modes
Configure in: `GLOBAL SETTINGS > SYSTEM > USB CONFIG`

| Mode | Description |
|------|-------------|
| **OVERBRIDGE** | Full Overbridge functionality (VST/AU plugin mode) |
| **USB MIDI** | MIDI only over USB |
| **USB AUDIO/MIDI** | Class compliant audio + MIDI (no drivers needed) |

**Note:** Only one mode can be active at a time

### Class Compliant Mode Features
When in USB AUDIO/MIDI mode:
- Stream audio and MIDI simultaneously over USB
- Works with computers, phones, tablets (no drivers required)
- Configure audio routing in `GLOBAL SETTINGS > AUDIO ROUTING`
- Can route audio to/from individual tracks
- Can process external audio through Analog Rytm effects

## Velocity & Aftertouch

### Velocity
Configure in: `SOUND MENU > SOUND SETTINGS`

**VELOCITY TO VOL**: How velocity affects volume
- **OFF**: Velocity doesn't affect volume
- **1-127**: Scaling amount

**VELOCITY MOD**: Assign up to 4 parameters to velocity modulation
- Open menu with `[YES]`
- Use DATA ENTRY knobs A-D to select parameters
- Use DATA ENTRY knobs E-H to set modulation depth
- Use TRACK LEVEL knob to set velocity range

### Aftertouch (Pressure)
**AFTERTOUCH**: Assign up to 4 parameters to pad pressure
- Same configuration interface as velocity mod
- Works with pads and external MIDI aftertouch

## Step Recording MIDI

When in STEP RECORDING mode:
- External MIDI controller must send on **AUTO CHANNEL**
- Velocity from MIDI controller is captured and parameter locked
- Hold `[FUNC]` while inputting to use fixed velocity instead
- Hold `[YES]` while placing trig to lock trig length

## Practical Examples

### Example 1: Control from DAW
```
1. Set INPUT FROM to USB
2. Enable CLOCK RECEIVE
3. Enable TRANSPORT RECEIVE
4. Enable RECEIVE NOTES
5. Set track MIDI channels in CHANNELS menu
6. Send MIDI notes 0-11 to trigger tracks
7. Send MIDI notes 12-59 to play active track chromatically
8. Send Program Changes to select patterns
```

### Example 2: Control External Synth
```
1. Set OUTPUT TO to MIDI
2. Enable CLOCK SEND
3. Enable TRANSPORT SEND
4. In TRACK MENU > TRIGS SETUP, enable TRK SEND MIDI for desired track
5. Sequencer will send note/velocity data on track's MIDI channel
```

### Example 3: Sync with Vintage Gear
```
1. Connect MIDI OUT to vintage device's SYNC IN
2. Set OUT PORT FUNCTIONALITY to DIN24 or DIN48
3. Enable CLOCK SEND
4. Vintage device will sync to Analog Rytm tempo
```

## Limitations & Notes

### Voice Stealing
Some tracks share voices:
- RS/CP (tracks 3/4)
- MT/HT (tracks 7/8)
- CH/OH (tracks 9/10)
- CY/CB (tracks 11/12)

Right-hand track has priority. If both are triggered simultaneously, the right track will mute the left.

### Missing Documentation
The manual references **APPENDIX C: MIDI** for complete CC/NRPN specifications, but this appendix is not included in the available documentation. To discover CC/NRPN mappings:
- Use MIDI monitoring software
- Enable PARAM OUTPUT (CC or NRPN mode)
- Turn knobs and observe sent MIDI messages
- Or check Elektron forums/community resources

### Sample Transfer
- Samples cannot be transferred via MIDI SysEx
- Use Elektron Transfer software (free download)
- Alternatively: MIDI Sample Dump Standard (SDS) via C6 utility (legacy method)

## Quick Reference Table

| Function | MIDI Message Type | Range | Notes |
|----------|-------------------|-------|-------|
| Trigger Track 1-12 | Note On | 0-11 (C0-B0) | Triggers specific track |
| Chromatic Play | Note On | 12-59 (C1-B4) | Active track only |
| Pattern Select | Program Change | 0-127 | Selects A01-H16 |
| Parameters | CC or NRPN | Various | Mappings not documented |
| Clock/Sync | MIDI Clock | - | 24 ppqn |
| Transport | Start/Stop/Continue | - | Standard MIDI transport |

## Additional Resources

- **Elektron Transfer**: Free software for sample/backup management
- **Elektron C6**: Free SysEx utility for backups
- **Overbridge**: VST/AU plugin for DAW integration
- Download from: https://www.elektron.se/support

---

*Last Updated: 2026-01-27*
*Based on: Analog Rytm MKII OS version 1.72 User Manual*
