# rytmloop 2.0 Specification

## Overview

The `rytmloop-2.0` format extends `rytmloop-1.0` to support complete song composition for the Elektron Analog Rytm MKII. It adds:

- **Scenes** (12 scene pads with absolute parameter locks)
- **Performances** (12 perf pads with delta modulation)
- **Song arrangement** (pattern chains with repeats and per-pattern mutes)
- **Trig conditions** (probability, fill, pre, nei, 1st, A:B ratios)
- **Parameter locks (P-Locks)** per step
- **Full machine parameter mapping** (33+ machines with named parameters)

## File Structure

```json
{
  "version": "rytmloop-2.0",
  "meta": {
    "title": "Track Title",
    "artist": "Artist Name",
    "bpm": 120,
    "created": "2025-01-28T12:00:00Z"
  },
  "kit": { /* kit settings */ },
  "patterns": { /* named patterns */ },
  "scenes": [ /* scene definitions */ ],
  "performances": [ /* performance definitions */ ],
  "song": { /* song arrangement */ }
}
```

## Backward Compatibility

Files using `rytmloop-1.0` format (single `tracks` array) are still valid. The loader detects format by:
- `"version": "rytmloop-2.0"` + `patterns` object = v2.0 multi-pattern
- `"version": "rytmloop-1.0"` or `tracks` array = v1.0 single pattern

---

## 1. Patterns

### 1.1 Multi-Pattern Structure

```json
{
  "patterns": {
    "intro": {
      "length": 16,
      "tracks": [ /* 12 track objects */ ]
    },
    "verse": {
      "length": 32,
      "tracks": [ /* 12 track objects */ ]
    },
    "drop": {
      "length": 64,
      "tracks": [ /* 12 track objects */ ]
    }
  }
}
```

### 1.2 Track Object (same as v1.0, extended with conditions and p-locks)

```json
{
  "name": "BD",
  "machine": "bdHard",
  "steps": [
    {
      "idx": 0,
      "events": [{
        "velocity": 127,
        "lengthSteps": 1,
        "note": 60,
        "condition": null,
        "locks": {}
      }]
    }
  ]
}
```

---

## 2. Trig Conditions

### 2.1 Condition Values

Each step event can have a `condition` field:

| Condition | Description |
|-----------|-------------|
| `null` | Always play (default) |
| `"1%"` - `"100%"` | Probability (e.g., `"50%"` = 50% chance) |
| `"1:2"` - `"8:8"` | Pattern cycle ratio (A:B = play on Ath of every B cycles) |
| `"fill"` | Only triggers when FILL mode is active |
| `"!fill"` | Only triggers when FILL mode is NOT active |
| `"pre"` | Triggers if previous trig condition on this track was TRUE |
| `"!pre"` | Triggers if previous trig condition on this track was FALSE |
| `"nei"` | Triggers if most recent trig on neighbor track (N-1) was TRUE |
| `"!nei"` | Triggers if most recent trig on neighbor track was FALSE |
| `"1st"` | Only triggers on first pattern loop |
| `"!1st"` | Triggers on all loops EXCEPT the first |

### 2.2 Pattern Cycle Ratios

Full list of A:B conditions:
- `"1:2"`, `"2:2"`
- `"1:3"`, `"2:3"`, `"3:3"`
- `"1:4"`, `"2:4"`, `"3:4"`, `"4:4"`
- `"1:5"`, `"2:5"`, `"3:5"`, `"4:5"`, `"5:5"`
- `"1:6"`, `"2:6"`, `"3:6"`, `"4:6"`, `"5:6"`, `"6:6"`
- `"1:7"`, `"2:7"`, `"3:7"`, `"4:7"`, `"5:7"`, `"6:7"`, `"7:7"`
- `"1:8"`, `"2:8"`, `"3:8"`, `"4:8"`, `"5:8"`, `"6:8"`, `"7:8"`, `"8:8"`

### 2.3 Fill Mode Design Pattern

```json
{
  "tracks": [{
    "name": "BD",
    "steps": [
      { "idx": 0, "events": [{ "velocity": 127 }] },
      { "idx": 4, "events": [{ "velocity": 127 }] },
      { "idx": 8, "events": [{ "velocity": 127 }] },
      { "idx": 12, "events": [{ "velocity": 127 }] },
      { "idx": 14, "events": [{ "velocity": 100, "condition": "fill" }] },
      { "idx": 15, "events": [{ "velocity": 90, "condition": "fill" }] }
    ]
  }]
}
```

**Workflow:**
1. Normal playback: steps 0,4,8,12 play (4-on-floor)
2. Hold FILL button: steps 14,15 ALSO play (drum fill)
3. Release FILL: back to normal

---

## 3. Parameter Locks (P-Locks)

### 3.1 Per-Step Locks

Any step event can lock parameters to specific values:

```json
{
  "idx": 4,
  "events": [{
    "velocity": 127,
    "condition": "fill",
    "locks": {
      "filter.frequency": 90,
      "filter.resonance": 80,
      "amp.decay": 40,
      "bdHard.snap": 100
    }
  }]
}
```

### 3.2 Lockable Parameters

P-locks can target ANY parameter:

- **Machine-specific SRC params**: `bdHard.snap`, `sdClassic.noiseMix`, etc.
- **Sample params**: `sample.tune`, `sample.start`, `sample.end`
- **Filter params**: `filter.frequency`, `filter.resonance`, `filter.envDepth`
- **Amp params**: `amp.decay`, `amp.pan`, `amp.volume`, `amp.overdrive`
- **LFO params**: `lfo.speed`, `lfo.depth`, `lfo.dest`
- **FX params**: `delay.feedback`, `reverb.decay`, etc. (on FX track)

---

## 4. Scenes

### 4.1 Scene Structure

```json
{
  "scenes": [
    {
      "id": 1,
      "name": "Build",
      "locks": [
        { "track": "BD", "param": "filter.frequency", "value": 60 },
        { "track": "CH", "param": "amp.volume", "value": 127 },
        { "track": "FX", "param": "delay.feedback", "value": 100 }
      ]
    },
    {
      "id": 2,
      "name": "Drop",
      "locks": [
        { "track": "BD", "param": "filter.frequency", "value": 127 },
        { "track": "SD", "param": "amp.reverbSend", "value": 40 }
      ]
    }
  ]
}
```

### 4.2 Scene Constraints

- **Maximum scenes**: 12 (id: 1-12)
- **Maximum locks**: 48 TOTAL shared across all scenes
- **Value type**: Absolute (0-127)
- **FX track**: Uses `delay.*`, `reverb.*`, `distortion.*`, `compressor.*`

---

## 5. Performances

### 5.1 Performance Structure

```json
{
  "performances": [
    {
      "id": 1,
      "name": "Filter Sweep",
      "locks": [
        { "track": "BD", "param": "filter.frequency", "delta": 50 },
        { "track": "SD", "param": "filter.frequency", "delta": 50 },
        { "track": "*", "param": "amp.reverbSend", "delta": 30 }
      ]
    }
  ]
}
```

### 5.2 Performance Constraints

- **Maximum perfs**: 12 (id: 1-12)
- **Maximum locks**: 48 TOTAL shared across all perfs
- **Delta type**: Signed (-128 to +127), added to current value
- **Wildcard**: `"track": "*"` applies to all drum tracks (1-12)

---

## 6. Song Arrangement

### 6.1 Song Structure

```json
{
  "song": {
    "name": "Deep Warehouse",
    "rows": [
      {
        "patterns": ["intro"],
        "repeat": 2,
        "mutes": {}
      },
      {
        "patterns": ["verse", "verse_var"],
        "repeat": 4,
        "mutes": {
          "verse_var": ["CY", "CB"]
        }
      },
      {
        "patterns": ["drop"],
        "repeat": 8,
        "mutes": {}
      }
    ]
  }
}
```

### 6.2 Song Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Song title |
| `rows` | array | Ordered list of song sections |
| `rows[].patterns` | string[] | Pattern names to chain |
| `rows[].repeat` | number | Times to repeat this row |
| `rows[].mutes` | object | Pattern name → muted track names |

---

## 7. Parameter Reference

### 7.1 Machine-Specific SRC Parameters

Each machine has 8 unique SRC page parameters:

#### Bass Drum Machines

| Machine | Parameters |
|---------|------------|
| `bdHard` | tune, sweepTime, snap, hold, decay, tick, wave, level |
| `bdClassic` | tune, sweepTime, sweepDepth, hold, decay, tone, hard, level |
| `bdFm` | tune, sweepTime, sweepDepth, fmDecay, fmDepth, decay, tone, level |
| `bdSharp` | tune, sweepTime, sweepDepth, tick, decay, tone, wave, level |
| `bdSilky` | tune, sweepTime, body, decay, dust, harmonics, tone, level |
| `bdAcoustic` | tune, sweepTime, body, decay, harmonics, wave, impact, level |

#### Snare Drum Machines

| Machine | Parameters |
|---------|------------|
| `sdHard` | tune, sweepTime, body, decay, noiseDecay, noiseMix, hardness, level |
| `sdClassic` | tune, sweepTime, body, decay, noiseDecay, noiseMix, tone, level |
| `sdNatural` | tune, sweepTime, body, decay, noiseDecay, noiseMix, ballSize, level |
| `sdFm` | tune, fmTune, fmDecay, fmDepth, noiseDecay, noiseMix, decay, level |
| `sdAcoustic` | tune, sweepTime, body, decay, snappy, tone, impact, level |

#### Rimshot/Clap Machines

| Machine | Parameters |
|---------|------------|
| `rsHard` | tune, sweepTime, decay, ringDecay, ringMix, tone, hardness, level |
| `rsClassic` | tune, sweepTime, decay, ringDecay, ringMix, tone, ring, level |
| `cpClassic` | tune, tone, hard, richness, rate, room, decay, level |

#### Cymbal Machines

| Machine | Parameters |
|---------|------------|
| `chClassic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `chMetallic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `ohClassic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `ohMetallic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `cyClassic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `cyMetallic` | tune, decay, tone, top, color, rate, toneDecay, level |
| `cyRide` | tune, decay, tone, top, color, rate, toneDecay, level |
| `cbClassic` | tune, decay, tone, snap, tone2, pulse, toneDecay, level |
| `cbMetallic` | tune, decay, tone, snap, tone2, pulse, toneDecay, level |
| `hhLab` | tune, decay, tone, harmonic, filter, chaos, attack, level |

#### Tom Machines

| Machine | Parameters |
|---------|------------|
| `btClassic` | tune, sweepTime, sweepDepth, hold, decay, tone, hard, level |
| `ltClassic` | tune, sweepTime, sweepDepth, hold, decay, tone, hard, level |
| `mtClassic` | tune, sweepTime, sweepDepth, hold, decay, tone, hard, level |
| `htClassic` | tune, sweepTime, sweepDepth, hold, decay, tone, hard, level |

#### Synth Machines

| Machine | Parameters |
|---------|------------|
| `syDualVco` | tune, detune, wave1, wave2, balance, bendDepth, bendTime, level |
| `syChip` | tune, wave, speed, arp, pulseWidth, pwmSpeed, pwmDepth, level |
| `syRaw` | tune, wave, detune, overtone, swarm, decay, roughness, level |

#### Utility Machines

| Machine | Parameters |
|---------|------------|
| `utNoise` | decay, lpf, hpf, attack, sweepTime, sweepDepth, color, level |
| `utImpulse` | attack, decay, freq, sweepTime, sweepDepth, color, filter, level |

**Generic access**: `synth.param1` through `synth.param8` for machine-agnostic control.

### 7.2 Common Track Parameters

#### TRIG Page
| Parameter | Range | Description |
|-----------|-------|-------------|
| `trig.note` | 0-127 | Note value (60=C4) |
| `trig.velocity` | 0-127 | Velocity |
| `trig.length` | 0-127, "inf" | Gate length |
| `trig.microTiming` | -23 to +23 | Micro timing offset |
| `trig.retrig` | bool | Retrig enable |
| `trig.retrigRate` | 1/1, 1/2, etc. | Retrig rate |
| `trig.retrigLength` | 0-127 | Retrig length |
| `trig.retrigVelocity` | 0-127 | Retrig velocity curve |

#### SMPL Page
| Parameter | Range | Description |
|-----------|-------|-------------|
| `sample.tune` | -64 to +63 | Sample pitch |
| `sample.fineTune` | -64 to +63 | Fine tuning |
| `sample.number` | 0-127 | Sample slot |
| `sample.bitReduction` | 0-127 | Bit reduction |
| `sample.start` | 0-127 | Start point |
| `sample.end` | 0-127 | End point |
| `sample.loop` | 0-127 | Loop point |
| `sample.level` | 0-127 | Sample level |

#### FLTR Page
| Parameter | Range | Description |
|-----------|-------|-------------|
| `filter.attack` | 0-127 | Filter env attack |
| `filter.decay` | 0-127 | Filter env decay |
| `filter.sustain` | 0-127 | Filter env sustain |
| `filter.release` | 0-127 | Filter env release |
| `filter.frequency` | 0-127 | Cutoff frequency |
| `filter.resonance` | 0-127 | Resonance |
| `filter.type` | 0-6 | Filter type |
| `filter.envDepth` | -64 to +63 | Envelope depth |

#### AMP Page
| Parameter | Range | Description |
|-----------|-------|-------------|
| `amp.attack` | 0-127 | Amp env attack |
| `amp.hold` | 0-127 | Amp env hold |
| `amp.decay` | 0-127 | Amp env decay |
| `amp.overdrive` | 0-127 | Overdrive amount |
| `amp.delaySend` | 0-127 | Delay send level |
| `amp.reverbSend` | 0-127 | Reverb send level |
| `amp.pan` | -64 to +63 | Pan position |
| `amp.volume` | 0-127 | Track volume |

#### LFO Page
| Parameter | Range | Description |
|-----------|-------|-------------|
| `lfo.speed` | 0-127 | LFO speed |
| `lfo.multiply` | 0-127 | Speed multiplier |
| `lfo.fade` | 0-127 | Fade in/out |
| `lfo.dest` | 0-127 | Destination parameter |
| `lfo.wave` | 0-6 | Waveform |
| `lfo.phase` | 0-127 | Start phase |
| `lfo.trigMode` | 0-3 | Trigger mode |
| `lfo.depth` | -128 to +127 | Modulation depth |

### 7.3 FX Track Parameters

#### Delay (Send Effect)
| Parameter | Range | Description |
|-----------|-------|-------------|
| `delay.time` | 0-127 | Delay time (tempo-synced) |
| `delay.pingPong` | 0-1 | Ping-pong mode |
| `delay.width` | 0-127 | Stereo width |
| `delay.feedback` | 0-127 | Feedback amount |
| `delay.hpf` | 0-127 | High-pass filter |
| `delay.lpf` | 0-127 | Low-pass filter |
| `delay.reverbSend` | 0-127 | Send to reverb |
| `delay.volume` | 0-127 | Delay volume |
| `delay.overdrive` | 0-127 | Overdrive |

#### Reverb (Send Effect)
| Parameter | Range | Description |
|-----------|-------|-------------|
| `reverb.preDelay` | 0-127 | Pre-delay time |
| `reverb.decay` | 0-127 | Decay time |
| `reverb.shelvFreq` | 0-127 | Shelving EQ frequency |
| `reverb.shelvGain` | 0-127 | Shelving EQ gain |
| `reverb.hpf` | 0-127 | High-pass filter |
| `reverb.lpf` | 0-127 | Low-pass filter |
| `reverb.volume` | 0-127 | Reverb volume |

#### Distortion (Master Effect)
| Parameter | Range | Description |
|-----------|-------|-------------|
| `distortion.amount` | 0-127 | Distortion amount |
| `distortion.symmetry` | 0-127 | Wave symmetry |

#### Compressor (Master Effect)
| Parameter | Range | Description |
|-----------|-------|-------------|
| `compressor.threshold` | 0-127 | Threshold |
| `compressor.attack` | 0-127 | Attack time |
| `compressor.release` | 0-127 | Release time |
| `compressor.ratio` | 0-127 | Compression ratio |
| `compressor.sideChainEq` | 0-127 | Sidechain EQ |
| `compressor.makeupGain` | 0-127 | Makeup gain |
| `compressor.dryWet` | 0-127 | Dry/wet mix |
| `compressor.volume` | 0-127 | Output volume |

---

## 8. Complete Example

```json
{
  "version": "rytmloop-2.0",
  "meta": {
    "title": "Warehouse Techno",
    "artist": "Claude",
    "bpm": 130,
    "created": "2025-01-28T12:00:00Z"
  },
  "kit": {
    "name": "Dark Kit"
  },
  "patterns": {
    "intro": {
      "length": 16,
      "tracks": [
        {
          "name": "BD",
          "machine": "bdHard",
          "sound": {
            "bdHard.tune": 64,
            "bdHard.decay": 80,
            "bdHard.snap": 60,
            "filter.frequency": 80,
            "amp.volume": 100
          },
          "steps": [
            { "idx": 0, "events": [{ "velocity": 127 }] },
            { "idx": 4, "events": [{ "velocity": 120 }] },
            { "idx": 8, "events": [{ "velocity": 127 }] },
            { "idx": 12, "events": [{ "velocity": 120 }] },
            { "idx": 14, "events": [{ "velocity": 100, "condition": "fill" }] },
            { "idx": 15, "events": [{ "velocity": 90, "condition": "fill" }] }
          ]
        },
        {
          "name": "SD",
          "machine": "sdHard",
          "steps": [
            { "idx": 4, "events": [{ "velocity": 110 }] },
            { "idx": 12, "events": [{ "velocity": 110 }] },
            { "idx": 10, "events": [{ "velocity": 80, "condition": "50%" }] }
          ]
        },
        {
          "name": "CH",
          "machine": "chClassic",
          "steps": [
            { "idx": 0, "events": [{ "velocity": 80 }] },
            { "idx": 2, "events": [{ "velocity": 70, "locks": { "filter.frequency": 60 } }] },
            { "idx": 4, "events": [{ "velocity": 80 }] },
            { "idx": 6, "events": [{ "velocity": 70, "locks": { "filter.frequency": 60 } }] },
            { "idx": 8, "events": [{ "velocity": 80 }] },
            { "idx": 10, "events": [{ "velocity": 70 }] },
            { "idx": 12, "events": [{ "velocity": 80 }] },
            { "idx": 14, "events": [{ "velocity": 70 }] }
          ]
        }
      ]
    },
    "drop": {
      "length": 16,
      "tracks": [
        {
          "name": "BD",
          "machine": "bdHard",
          "steps": [
            { "idx": 0, "events": [{ "velocity": 127, "locks": { "amp.overdrive": 80 } }] },
            { "idx": 4, "events": [{ "velocity": 127 }] },
            { "idx": 8, "events": [{ "velocity": 127 }] },
            { "idx": 12, "events": [{ "velocity": 127 }] }
          ]
        }
      ]
    }
  },
  "scenes": [
    {
      "id": 1,
      "name": "Build",
      "locks": [
        { "track": "BD", "param": "filter.frequency", "value": 40 },
        { "track": "CH", "param": "amp.volume", "value": 60 }
      ]
    },
    {
      "id": 2,
      "name": "Drop",
      "locks": [
        { "track": "BD", "param": "filter.frequency", "value": 127 },
        { "track": "FX", "param": "delay.feedback", "value": 90 }
      ]
    }
  ],
  "performances": [
    {
      "id": 1,
      "name": "Filter Sweep",
      "locks": [
        { "track": "*", "param": "filter.frequency", "delta": 40 }
      ]
    },
    {
      "id": 2,
      "name": "Reverb Wash",
      "locks": [
        { "track": "*", "param": "amp.reverbSend", "delta": 50 }
      ]
    }
  ],
  "song": {
    "name": "Warehouse Techno",
    "rows": [
      { "patterns": ["intro"], "repeat": 4, "mutes": {} },
      { "patterns": ["intro"], "repeat": 4, "mutes": { "intro": ["CH"] } },
      { "patterns": ["drop"], "repeat": 8, "mutes": {} }
    ]
  }
}
```

---

## 9. Validation Rules

1. **Version**: Must be `"rytmloop-2.0"` for v2 features
2. **Scene limits**: Max 12 scenes, max 48 locks total
3. **Performance limits**: Max 12 perfs, max 48 locks total
4. **Pattern references**: All patterns in `song.rows[].patterns` must exist in `patterns`
5. **Track names**: Must be valid: BD, SD, RS, CP, BT, LT, MT, HT, CH, OH, CY, CB, FX
6. **Parameter names**: Must match documented parameters or `synth.param1-8`
7. **Condition format**: Must match documented condition patterns

---

## 10. Migration from v1.0

To migrate a `rytmloop-1.0` file:

1. Change `"version"` to `"rytmloop-2.0"`
2. Wrap `"tracks"` array in a named pattern:
   ```json
   {
     "patterns": {
       "main": {
         "tracks": [ /* existing tracks */ ]
       }
     }
   }
   ```
3. Add `scenes`, `performances`, `song` as needed

**Note**: v1.0 files with just `"tracks"` array continue to work for backward compatibility.
