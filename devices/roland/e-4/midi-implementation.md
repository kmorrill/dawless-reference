# Roland AIRA Compact E-4 - External MIDI Control Reference

Source: Roland E-4 Owner's Manual v1.02.

---

## MIDI I/O (how you connect)

- USB-C: class-compliant USB MIDI (and USB audio).
- 3.5mm TRS MIDI IN / OUT: uses TRS<->TRS or TRS<->DIN cables (Roland lists BCC/BMIDI cable types).

---

## MIDI channel + mode

- Default channel: 3
- Changeable: Ch.1-Ch.16 (memorized)
- Where to set it: Menu -> MIDI CHANNEL (set with [SCATTER] dial)
- MIDI Mode: Mode 3 (Omni off, Poly)

---

## What the E-4 responds to (MIDI IN / USB MIDI IN)

### 1) Note On/Off -> ROBOT NOTE (vocoder pitch / chord control)

- Recognizes Note On/Off
- Note number range: 0-127
- Manual calls this ROBOT NOTE: Note On/Off controls vocoder pitch using note numbers; [AUTO PITCH] blinks when notes are input.
- If [HARMONY] is lit, you can control up to 4 voices (chords).

KEY IN setup (so external notes actually drive the vocoder):

- Enter menu -> select KEY.I -> set MIDI channel with [SCATTER] -> exit menu -> then play using notes + pitch bend from external MIDI/USB.
- System setting also describes MIDI KEY IN = Off / KeY.I, where KeY.I means pitch remains fixed when note messages are input.

Related note:

- In the AUTO PITCH ROBOT behavior, the manual notes you can use an external MIDI device to input the key, setting the fixed pitch.

### 2) Pitch Bend -> controls the PITCH slider

- Manual: Pitch Bend controls the [PITCH] slider value.
- Implementation chart: Pitch Bend is recognized.

### 3) MIDI Clock + Transport -> looper sync (timing)

- Manual: Clock controls the looper timing when syncing.
- When connected via MIDI jacks or USB MIDI, synchronization begins when MIDI Start (FA) is received.
- Implementation chart: Clock / Start / Continue / Stop are recognized.
- Menu:
- MIDI CLOCK SYNC source: Auto / Internal / MIDI / USB
- SYNC RATE for looper cycle: 1/8, 1/4, 1/2, 1

### 4) MIDI THRU (routing)

- Menu: MIDI THROUGH can forward MIDI IN -> MIDI OUT (set to Through).

---

## What the E-4 does NOT support (important for your "everything" list)

### MIDI CC (Control Change)

- Not recognized, not transmitted (no CC control surface).

### Program Change

- Not recognized, not transmitted.

### SysEx

- Not recognized, not transmitted.

### System Common (Song Position / Song Select / Tune Request)

- Not supported.

---

## What the E-4 transmits (useful for wiring rigs)

From the MIDI implementation chart, the E-4 transmits:

- Clock, Start, Stop
- All Sound Off and Active Sensing

And it does not transmit Note messages / CC / Program Change / SysEx.
