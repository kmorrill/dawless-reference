# Teenage Engineering Pocket Operators

## Overview
- **Type**: Ultra-compact battery-powered synths/samplers/drum machines
- **Models**: 15+ models across multiple series (PO-12 rhythm, PO-20 arcade, PO-33 K.O!, PO-35 speak, etc.)
- **Sequencer**: 16 steps, 16 patterns
- **Power**: 2x AAA batteries

## Connectivity
| Interface | Direction | Details |
|-----------|-----------|---------|
| 3.5mm (left) | In/Out | Audio in + sync in |
| 3.5mm (right) | Out | Audio out + sync out |

No USB. No MIDI (native). No Bluetooth.

## Sync Protocol
Pocket Operators sync via a **voltage pulse on the 3.5mm audio cable**:

- **Sync out**: Sends a click/pulse on each step (right channel or shared)
- **Sync in**: Receives pulse to advance sequencer
- **Sync modes** (per device):
  - SY0: No sync
  - SY1: Sync receive (follow external clock)
  - SY2: Sync send (master clock)
  - SY3: Sync send + receive (pass-through)
  - SY4: Sync receive, half speed
  - SY5: Sync send, half speed

### Daisy-chaining
Multiple POs can be daisy-chained via 3.5mm stereo cables. Audio and sync travel on the same cable:
- Left channel: audio + sync in
- Right channel: audio + sync out

## MIDI (via adapter)
- **PO-33 K.O!** and some others can receive MIDI note input via the 3.5mm sync port using the TE MIDI adapter or third-party adapters
- Very limited: note-on only, no CC, no program change
- Adapter converts MIDI to the PO's sync/audio protocol

## Models by Series

### Pocket Operator Series
| Model | Type |
|-------|------|
| PO-12 | Rhythm (drum machine) |
| PO-14 | Sub (bass synth) |
| PO-16 | Factory (melody synth) |
| PO-20 | Arcade (chiptune) |
| PO-24 | Office (noise) |
| PO-28 | Robot (lead synth) |
| PO-32 | Tonic (drum synth, microtonic) |
| PO-33 | K.O! (sampler) |
| PO-35 | Speak (vocal synth) |

## Limitations
- No screen beyond 2-digit 7-segment LED
- No MIDI without adapter (and then very limited)
- Exposed circuit board (fragile without case)
- Battery powered only
- 16-step sequencer only
