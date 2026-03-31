# OP-XY Drum Note Reference (Working)

## Purpose

Collect the current note mappings used for OP-XY drum patterns and drum kit slots.
This is a working reference assembled from local repos and should be verified on-device.

## Sources

- `~/Documents/op-xy-vibing/tmp/external/logic_pro_drums_for_opxy/splice_and_export.py`
- `~/Documents/op-xy-vibing/tmp/external/OP-XY-Drum-Utility/patch.json`
- `~/Documents/op-xy-live/src/devices/op-xy.js` (prototype map)
- `~/Documents/op-xy-vibing/musical-coding-assistant.md` (GM-style examples)

## 24-slot drum kit range (OP-XY drum patches)

Slot notes are defined as MIDI 53-76 (24 slots). This aligns with the regions in
`OP-XY-Drum-Utility/patch.json` and `OPXY_SLOT_NOTES` in the Logic export tool.

Note numbers and names (standard MIDI mapping):

- 53: F3
- 54: F#3
- 55: G3
- 56: G#3
- 57: A3
- 58: A#3
- 59: B3
- 60: C4
- 61: C#4
- 62: D4
- 63: D#4
- 64: E4
- 65: F4
- 66: F#4
- 67: G4
- 68: G#4
- 69: A4
- 70: A#4
- 71: B4
- 72: C5
- 73: C#5
- 74: D5
- 75: D#5
- 76: E5

Note: The Logic tool comment says F#3 to F5, but MIDI 53-76 corresponds to F3 to E5.
Treat the numeric range as authoritative.

## Role mapping (prototype, op-xy-live)

The op-xy-live prototype assigns roles to the 53-76 slot range:

- 53: kick
- 54: kick_alt
- 55: snare
- 56: snare_alt
- 57: rim
- 58: clap
- 59: tambourine
- 60: shaker
- 61: closed_hat
- 62: open_hat
- 63: pedal_hat
- 65: low_tom
- 66: crash
- 67: mid_tom
- 68: ride
- 69: high_tom
- 71: conga_low
- 72: conga_high
- 73: cowbell
- 74: guiro
- 75: metal
- 76: chi

Gaps in this map: 64, 70 are unused in the prototype.

## GM-style drum map (example)

Some OP-XY docs reference a GM-style drum map for editing standard drum tracks:

- Kick: 36
- Snare: 38
- Closed hat: 42
- Open hat: 46
- Tom: 45
- Rim: 49

Use this map only if the OP-XY is set up for GM-style drum note mapping.
