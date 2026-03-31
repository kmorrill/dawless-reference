# CC 106/107 — Remote Key Press

Two MIDI CCs control key presses on the OP-XY:
- **CC 106** = key down (press)
- **CC 107** = key up (release)

Same value maps to the same key on both CCs.  This is a true down/up
mechanism — CC 106 alone leaves the key held until CC 107 releases it.

Discovered 2026-03-04 via systematic sweep over CC 106 values 0–127.
CC 107 release mechanism confirmed same session.

## Hardware Setup

MIDI input to OP-XY is via **USB-C** (the AUX Type A port is output-only).
The device appears as `OP-XY` in the MIDI port list.

## Value Map

### Mode / Navigation (0–13)

| CC Value | Key | Notes |
|----------|-----|-------|
| 0 | Project | Opens project browser |
| 1 | Tempo | Opens tempo/swing screen |
| 2 | Instrument | Press-and-hold behavior (synth/drum tracks) |
| 3 | Auxiliary | Press-and-hold behavior (aux tracks) |
| 4 | Arranger | Opens arranger view |
| 5 | Mixer | Opens mixer view |
| 6 | M1 | Sub-mode button 1 (below screen) |
| 7 | M2 | Sub-mode button 2 |
| 8 | M3 | Sub-mode button 3 |
| 9 | M4 | Sub-mode button 4 |
| 10 | ? | Unknown — no observed effect |
| 11 | ? | Unknown — no observed effect |
| 12 | ? | Unknown — no observed effect |
| 13 | ? | Unknown — no observed effect |

### Track Select (14–21)

| CC Value | Key | Notes |
|----------|-----|-------|
| 14 | Track 1 | Drum (default) |
| 15 | Track 2 | Drum (default) |
| 16 | Track 3 | Prism (default) |
| 17 | Track 4 | EPiano (default) |
| 18 | Track 5 | Dissolve (default) |
| 19 | Track 6 | Hardsync (default) |
| 20 | Track 7 | Axis (default) |
| 21 | Track 8 | Multisampler (default) |

### Special Keys (22–25)

| CC Value | Key | Notes |
|----------|-----|-------|
| 22 | Player | Arp/player on/off toggle |
| 23 | Sample | Opens sample screen |
| 24 | Com | Opens communication/settings |
| 25 | Bar | Opens bar count menu |

### Keyboard (26–49)

24 keys spanning F to E (two octaves), matching the OP-XY's physical keyboard.

| CC Value | Key |
|----------|-----|
| 26 | F (first/lowest key) |
| 27–48 | F# through D# |
| 49 | E (last/highest key) |

Keyboard keys play notes audibly and register as the "most recently played
note" for sequencing purposes.

### Transport (50–55)

| CC Value | Key | Notes |
|----------|-----|-------|
| 50 | Record | Hold for record mode; hold while paused for step record |
| 51 | Play | Start playback |
| 52 | Stop | Stop playback |
| 53 | Minus (−) | Lower octave / step back in step record mode |
| 54 | Plus (+) | Raise octave / skip step in step record mode |
| 55 | Shift | Access secondary functions |

### Sequencer Steps (56–71)

| CC Value | Step |
|----------|------|
| 56 | Step 1 |
| 57 | Step 2 |
| 58 | Step 3 |
| 59 | Step 4 |
| 60 | Step 5 |
| 61 | Step 6 |
| 62 | Step 7 |
| 63 | Step 8 |
| 64 | Step 9 |
| 65 | Step 10 |
| 66 | Step 11 |
| 67 | Step 12 |
| 68 | Step 13 |
| 69 | Step 14 |
| 70 | Step 15 |
| 71 | Step 16 |

### Values 72–127

No observed effect.

## Layout Reference

The CC values map across the OP-XY panel:

```
Row 1 (top):     [Project] [Tempo] [Instrument] [Aux]  ...encoders...  [Sample] [Com]
                    CC 0     CC 1      CC 2      CC 3                    CC 23    CC 24

Row 2:           [Inst1-8] [Aux1-8] [Mixer] [T1] [T2] [T3] [T4] [T5] [T6] [T7] [T8] [Arranger] [Bar]
                                     CC 5   CC14  CC15 CC16 CC17 CC18 CC19 CC20 CC21    CC 4      CC25

Row 3 (seq):     [S1] [S2] [S3] [S4] [S5] [S6] [S7] [S8] [S9] [S10] [S11] [S12] [S13] [S14] [S15] [S16]
                 CC56  CC57 CC58 CC59 CC60 CC61 CC62 CC63 CC64  CC65  CC66  CC67  CC68  CC69  CC70  CC71

Row 4 (number):  [Rec] [Play] [Num] [1] [2] [3]  [4]  [5]  [6]  [7]  [8]  [9]  [0]
                 CC50   CC51         ← keyboard keys CC 26–49 span rows 4+5 →

Row 5 (bottom):  [−] [+] [Shift] [...F-keys / keyboard continued...]
                CC53 CC54  CC55

                           [Player]  = CC 22
                           [Stop]    = CC 52
                           [M1–M4]   = CC 6–9
```

## Confirmed Behavior (2026-03-04)

### Key Down / Key Up Protocol

- **CC 106** sends key down.  The key stays held until **CC 107** releases it.
- Sending CC 106 twice does NOT toggle — it triggers the key again (e.g.,
  keyboard keys re-trigger the note, staying held).
- All keys use this protocol: navigation, tracks, keyboard, transport,
  sequencer steps.

### Tap Pattern

A "tap" = CC 106 (down) → sleep → CC 107 (up).  500ms hold works reliably
for most keys.  The tool's `tap` command automates this.

### New Project

```
tap project 0.5    # open project browser
wait 0.5
tap m1 3.0         # M1 = "new" — hold time doesn't matter but 3s is safe
```

Creates a new unnamed project.  M1 triggers instantly via CC despite
requiring a physical hold on the device.

### Track Selection

```
tap track3 0.5     # select track 3 (Prism)
```

**Warning**: Holding a track key while pressing another track key **links
tracks** (up to 4).  Always release track keys promptly.

### Step Sequencing — Direct Step Entry

**Does NOT work via CC 106/107.**  The physical workflow (tap note → tap
step) does not produce sequenced notes when driven via CC.  The step key
activates visually but the note is not committed.  Likely cause: CC 107
release on the keyboard key clears the "most recently played note" register
before the step key is pressed.

### Step Sequencing — Step Record Mode (WORKING)

Hold record while stopped to enter step record mode.  Notes played on the
keyboard are automatically placed on sequential steps.  Use plus (+) to skip
steps.

```
tap stop 0.5           # ensure stopped
wait 0.5
down record            # hold record = step record mode
wait 1.0
tap c4 0.5             # note on step 1
wait 0.5
tap plus 0.5           # skip step 2
wait 0.5
tap e4 0.5             # note on step 3
wait 0.5
up record              # exit step record
```

**Confirmed working** — notes are sequenced and persist.  Tested with
melodies spanning arbitrary steps across all 16 positions.

### Live Recording (WORKING)

Hold record during playback.  Notes are captured in real time with their
actual durations.

```
tap play 0.5           # start playback
wait 1.0
down record            # hold record
wait 0.5
down c4                # hold note (plays for duration of hold)
wait 0.3
up c4                  # release note
wait 1.0
down e4
wait 0.8
up e4
wait 1.0
up record              # stop recording
wait 0.5
tap stop 0.5           # stop playback
```

**Confirmed working** — notes are recorded with varying gate lengths
matching the CC 106→107 hold duration.

### Note Ties

Per the TE manual: hold a sequencer key, press a later sequencer key, then
release both.  The note extends from the first to the second step.
Untested via CC.

## Tool

`tools/cc106_keys.py` — CLI for CC 106/107 key control.

```bash
# List all key names and CC values
python tools/cc106_keys.py list

# Tap a key (down + hold + up)
python tools/cc106_keys.py --port OP-XY tap project --hold 0.5

# Tap multiple keys in sequence
python tools/cc106_keys.py --port OP-XY tap project m1 track3 --hold 0.5

# Key down / up separately
python tools/cc106_keys.py --port OP-XY down record
python tools/cc106_keys.py --port OP-XY up record

# Run a script file
python tools/cc106_keys.py --port OP-XY run scripts/fresh_step_record.txt
```

### Script Format

```
tap project 0.5        # tap key with hold duration
tap m1 3.0             # longer hold
down record            # key down only
up record              # key up only
wait 1.0               # pause in seconds
# this is a comment
```

Key names: `project`, `tempo`, `instrument`, `aux`, `arranger`, `mixer`,
`m1`–`m4`, `track1`–`track8` (or `t1`–`t8`), `player`, `sample`, `com`,
`bar`, `record`, `play`, `stop`, `minus`, `plus`, `shift`, `s1`–`s16`,
and note names (`c4`, `f#3`, `bb4`, etc.).

## Open Questions

- **CC 10–13**: No observed effect.  Possibly reserved or encoder-related.
- **Shift combos**: Can CC 106=55 (shift down) be held while tapping another
  key to trigger shifted functions (e.g., shift+track = preset select)?
- **Direct step entry**: Why doesn't tap-note-then-tap-step work via CC?
  The note plays and the step activates but nothing is committed.
- **Note ties via CC**: Hold step key, press later step key — untested.
- **Encoder control**: No CC discovered for encoder rotation yet.

## Implications for Automation

CC 106/107 combined with CC 86 (project select) enables:

1. **Project creation**: new project via `project` → `m1`
2. **Track selection**: `track1`–`track8`
3. **Step record programming**: hold `record`, tap notes + `plus` to skip
4. **Live recording**: hold `record` during playback, play notes with timing
5. **Transport control**: play, stop, record
6. **Multi-track compositions**: repeat step-record per track

This is a viable path for programmatic project creation on the device,
bypassing binary format authoring for test cases.
