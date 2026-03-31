# Roland AIRA Compact **J-6** — MIDI IN Control Reference (External Control)

## MIDI ports

- **USB-C (class-compliant):** USB MIDI (and USB audio).
- **TRS MIDI IN/OUT (3.5mm “stereo miniature”):** for external MIDI devices.

---

## Channel layout

The J-6 effectively uses **two MIDI channels**: one for synth notes, and one dedicated to program-change (pattern switching).

| Function                                      | Default | Range / Setting   |
| --------------------------------------------- | ------: | ----------------- |
| Notes (synth) RX/TX channel                   |       1 | **CH** = 1-16     |
| Program Change (pattern select) RX/TX channel |      16 | **Pc.Ch** = 1-16  |

Defaults and range are also shown in the MIDI Implementation Chart (`1 (Synth), 16 (PC)`, changed `1-16`).

---

## Notes (what you can “play” from external MIDI)

**Recognized:**

- **Note On / Note Off:** Yes
- **Note numbers:** 0-127
- **Velocity:** Yes

**Not recognized:**

- **Pitch Bend:** No
- **Aftertouch (key/channel):** No

---

## MIDI CC (Control Change)

The implementation chart shows **only one inbound CC**:

| CC # | Name on J-6 | Recognized? | Notes                                  |
| ---: | ----------- | :---------: | -------------------------------------- |
|   64 | **Hold**    |      Yes    | Sustain/hold behavior (labeled `Hold`) |

Everything else is effectively unsupported via CC (only `CC64` is listed).

---

## Program Change (pattern switching)

**Recognized:** Program Change **0-63**

To make inbound PC actually switch patterns:

- **rxPc (RX Program Change) must be ON** (`pattern changes when a program change message is received`).
- Choose the **Pc.Ch** channel used for PC messages.

Related settings that matter in practice:

- **P.chg (Pattern Change timing):** sets when pattern changes occur (end of step vs after a certain number of beats).
- **txPc (TX Program Change):** whether the J-6 sends PC when the pattern changes.

*Mapping note:* the manual confirms the accepted PC range (`0-63`) but does not spell out the exact number-to-pattern mapping. In practice this is typically `0` = first pattern through `63` = last pattern, but verify on your setup.

---

## Sync + transport (clocked playback)

**Recognized (System Real Time):**

- **MIDI Clock:** Yes
- **Start:** Yes
- **Stop:** Yes
- **Continue:** No

Clock-source selection (helpful when troubleshooting):

- **SYnC (MIDI Clock Sync):** `AUTO` / `Int` / `MIDI` / `USB`.

---

## “Utility” MIDI messages (Aux)

Recognized (inbound):

- **All Sound Off:** Yes
- **Reset All Controllers:** Yes
- **All Notes Off:** Yes
- **Active Sensing:** Yes

Not recognized:

- Omni On/Off, Mono Mode On, Poly Mode On, System Reset: No

(Those are listed in the chart but not supported on RX.)

---

## SysEx / System Common

Not supported on MIDI IN:

- **System Exclusive (SysEx):** No
- **Song Position / Song Select / Tune Request:** No

---

## MIDI routing / quality-of-life settings (device menus)

- **thrv (MIDI Thru):** pass MIDI IN -> MIDI OUT.
- **CH (MIDI Channel):** sets synth note RX/TX channel.
- **rxPc / txPc / Pc.Ch:** enable PC receive/transmit and choose the PC channel.
