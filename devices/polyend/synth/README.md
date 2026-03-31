# Polyend Synth

This repo can target the **Polyend Synth** as a MIDI destination (alongside OP-XY or other devices) using per-track MIDI port routing, Program Changes, and CC automation. The Polyend Synth has engine-specific CC maps, per-slot MIDI channels, and scene-level program change mapping, so a little setup goes a long way.

Source reference: `op-xy-live/docs/polyend-synth-midi.md` (Polyend Synth Manual 2v0a notes).

## Routing tracks to the Polyend Synth

Set `track.portName` to a substring that matches your Polyend Synth MIDI output port name (as returned by `mido.get_output_names()`), for example:

```json
{
  "id": "t-poly",
  "name": "Polyend Pad",
  "type": "ACD",
  "device": "polyend_synth",
  "portName": "Polyend Synth",
  "midiChannel": 0,
  "pattern": { "lengthBars": 1, "steps": [] }
}
```

Tracks without `track.portName` fall back to `deviceProfile.portName`, and then to the Conductor `--port` flag.

## Supported incoming MIDI on Polyend Synth

Polyend Synth accepts:
- Note On/Off
- Velocity (macro source; default routing is volume)
- Aftertouch / Channel Pressure (macro source)
- Pitch Bend
- CC (Control Change)
- PC (Program Change) for preset switching (scene mapping)

Not supported: SysEx, NRPN.

## MIDI settings on the Polyend Synth

All MIDI configuration lives in Main Menu -> MIDI.

### Clock In / Transport In
- Clock In: Internal (default) or External via USB or MIDI IN.
- Transport In: Off (default) or USB/MIDI IN.
- Wait for Start: if enabled, the arp/seq only runs after MIDI Start.

Implementation note: the synth uses internal timing at 96 PPQN, but MIDI clock is 24 PPQN.

### Per-slot MIDI channel + Local Mode
Each of the 3 synth slots has:
- A MIDI channel (target slot 1 vs 2 vs 3)
- Local Mode
  - ON: pads + MIDI both play the slot
  - OFF: pads disconnected; external MIDI only

### External keyboard pad mapping
Incoming note mapping can be Off, All, or White Keys, with a Base Note for alignment.

## Targeting the 3 synth engines (repo behavior)

Send MIDI to the slot's configured MIDI channel.

Implementation note: engine-specific CC lookup is keyed by MIDI channel in `src/devices/polyend-synth.js`. If your slot/channel assignment differs, update it via `deviceDefinition.setSynthEngine(channel, { name: "ACD", type: "..." })` or `liveLoop.setEngine("ACD")`.

## Program Change support

- PC 0-7 maps to 8 preset slots (scene-level mapping)
- Configure in Main Menu -> Scene Settings -> Program Change Mapping
- Preset must match the engine loaded in that slot
- Some devices count PCs from 1; support a `pcBase = 0|1` offset

Example:

```json
{
  "program": { "program": 10, "bankMsb": 1, "bankLsb": 2 },
  "programChanges": [
    { "t": { "bar": 0, "step": 8 }, "program": 11 }
  ]
}
```

## CC automation and parameter names

You can always target CCs directly via `cc:<number>` or an integer `dest`:

```json
{ "id": "cut", "dest": "cc:74", "points": [{ "t": { "bar": 0, "step": 0 }, "v": 64 }] }
```

If `track.device` is set to `polyend_synth`, `name:<id>` destinations resolve using the built-in Polyend Synth CC map. Example mappings:
- `name:cutoff` -> CC74
- `name:resonance` -> CC71
- `name:amp_attack` -> CC75
- `name:filter_attack` -> CC80

The current full map is in `conductor/devices/polyend_synth.py`.

### Shared CCs (common across engines)

Filter:
- CC 74 = Filter Cutoff
- CC 71 = Filter Resonance
- CC 77 = Filter Env Amount
- CC 78 = Filter LFO (where applicable)

Amp Envelope (Env Amp):
- CC 75 = Attack
- CC 72 = Decay
- CC 76 = Sustain
- CC 73 = Release

Filter Envelope (Env Filter):
- CC 80 = Attack
- CC 81 = Decay
- CC 82 = Sustain
- CC 83 = Release

Aux Envelope (Env Aux, engines that support Aux only):
- CC 46 = Attack
- CC 47 = Decay
- CC 48 = Sustain
- CC 49 = Release

### Engine-specific CC highlights

ACD:
- CC 20 Saw Mix
- CC 21 Square Mix
- CC 22 Sub Mix
- CC 23 Noise
- CC 24 PW
- CC 25 PW LFO
- CC 26 PW Env
- CC 27 Pitch LFO

FAT:
- CC 20 Timbre
- CC 21 Noise
- CC 23 Brightness
- CC 24 Fatness LFO
- CC 27 Fatness
- CC 54 LFO Frequency

WAVS:
- CC 20 Mix
- CC 21 Warp 1
- CC 22 Position 1
- CC 25 Tune 1
- CC 26 Finetune
- CC 27 Position 2
- CC 28 Warp 2
- CC 29 Noise
- CC 30 Tune 2
- CC 31 Detune

VAP:
- CC 20 Mix
- CC 21 PW 1
- CC 22 Shape 1
- CC 23 Noise
- CC 25 Tune 1
- CC 26 Detune
- CC 27 Shape 2
- CC 28 PW 2
- CC 30 Tune 2
- CC 31 Finetune

WTFM:
- CC 20 FM
- CC 21 Feedback 1
- CC 22 Shape 1
- CC 23 Ratio 1
- CC 24 Ratio 2
- CC 25 Finetune 2
- CC 26 Finetune 1
- CC 27 Shape 2
- CC 28 Feedback 2
- CC 29 Feedback 2->1

PMD (Exciter):
- CC 20 Bow Level
- CC 21 Bow Timbre
- CC 22 Air Level
- CC 23 Air Flow
- CC 24 Air Timbre
- CC 25 Strike Level
- CC 26 Strike Mallet
- CC 27 Strike Timbre

PMD (Resonator):
- CC 70 Form
- CC 71 Damping
- CC 74 Brightness
- CC 85 Position
- CC 86 Space

PHZ:
- CC 20 Mix
- CC 21 Osc1 XMod
- CC 22 Shape 1
- CC 23 Osc1 YMod
- CC 24 Detune
- CC 25 Tune 1
- CC 26 Finetune
- CC 27 Shape 2
- CC 28 Osc2 XMod
- CC 29 Osc2 YMod
- CC 30 Tune 2

GRAIN (Engine page 1):
- CC 20 Position
- CC 21 Position Spread
- CC 22 Time Spread
- CC 23 Density
- CC 24 Grain Size
- CC 25 Grain Shape
- CC 27 Pan Spread

GRAIN (Engine page 2):
- CC 26 Detune Spread
- CC 30 Tune
- CC 31 Finetune
- CC 85 Space
- CC 86 Direction
- CC 87 Size Spread

## Parameter keys for automation (code-facing)

Use these keys with LFO `targetParam`, loop automation events (`param`), or `liveLoop.setParam`.

Common keys:
- filterCutoff, resonance, envAmount, filterLfo
- ampAttack, ampDecay, ampSustain, ampRelease
- filterAttack, filterDecay, filterSustain, filterRelease
- auxAttack, auxDecay, auxSustain, auxRelease (engines with Aux only)
- lfoFrequency (FAT only)

ACD:
- sawMix, squareMix, subMix, noise, pulseWidth, pwLfo, pwEnv, pitchLfo

FAT:
- timbre, noise, brightness, fatnessLfo, fatness, lfoFrequency

WAVS:
- mix, warp1, position1, tune1, finetune, position2, warp2, noise, tune2, detune

VAP:
- mix, pw1, shape1, noise, tune1, detune, shape2, pw2, tune2, finetune

WTFM:
- fm, feedback1, shape1, ratio1, ratio2, finetune2, finetune1, shape2, feedback2, feedback2to1

PMD:
- bowLevel, bowTimbre, airLevel, airFlow, airTimbre, strikeLevel, strikeMallet, strikeTimbre
- form, damping, brightness, position, space
- exciterAttack, exciterDecay, exciterSustain, exciterRelease
- ampAttack, ampDecay, ampSustain, ampRelease
- filterAttack, filterDecay, filterSustain, filterRelease

PHZ:
- mix, osc1XMod, shape1, osc1YMod, detune, tune1, finetune, shape2, osc2XMod, osc2YMod, tune2

GRAIN:
- position, positionSpread, timeSpread, density, grainSize, grainShape, panSpread
- detuneSpread, tune, finetune, space, direction, sizeSpread

## Scaling + implementation notes

- CC values are 0-127 and map into each parameter's range (assume linear unless tested otherwise).
- Unipolar: `value = min + (cc/127) * (max - min)`
- Bipolar: `value = -1 + 2 * (cc/127)`, then scale
- Enums: quantize CC into N bins

Controller layers should track which engine is loaded per slot, use the engine-specific CC map, and still allow shared CCs (filter/env) when present.

## Checklist

Per synth slot:
- MIDI channel set correctly
- Local Mode set as intended
- Notes trigger sound
- Velocity and aftertouch respond as intended
- Pitch bend behaves as expected
- CCs respond for filter/env and engine-specific params
- PC mapping set, PC 0-7 switches presets

Clock/transport:
- Clock In set to USB or MIDI IN
- Transport In set correctly
- Wait for Start set appropriately for the sender

## Appendix: MIDI Instrument engine

Polyend Synth includes a MIDI Instrument engine for controlling external gear by sending CCs from its knobs/pages. This is separate from controlling Synth itself but matters in controller-heavy rigs.
