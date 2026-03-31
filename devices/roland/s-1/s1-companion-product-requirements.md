# S-1 Companion Web App (Android Chrome + WebMIDI) — Product Requirements
Author: Kevin Morrill  
Target device: **Roland AIRA Compact S-1**  
Form factor: **Mobile-first PWA** running in **Chrome on Android** with **WebMIDI**  
Purpose: Eliminate **menu diving**, add **morphing + modulation**, and provide a **smarter sequencer** to supercharge the S-1.

---

## 1) Background and Intent
The Roland S-1 is powerful but has two practical limitations for creative sound design and performance:

1) **Deep / non-obvious controls** require SHIFT combos and/or menu navigation. This kills flow.
2) The device’s modulation and sequencing are capable, but limited for:
   - multiple simultaneous modulations per target
   - “patch morphing” performance
   - modern, ergonomic sequencing and parameter automation workflows

This companion app is meant to:
- keep the **hardware** as the tactile instrument
- move the **complexity** (and expressivity) to the phone:
  - macros that bundle multiple CC moves into “musical” controls
  - bookmarks and morph (Polybrute-like)
  - independent LFOs per target (overcoming single-LFO constraints)
  - a modern sequencer that is voice-aware and can intelligently use Poly/Chord/Unison

---

## 2) Product Principles
### 2.1 Design principles
- **No menu diving** during sound design/performance.
- **Music-first controls** > raw parameter grids.
- **High leverage**: 6–10 controls should cover 80% of creative needs.
- **Expressive gestures**: XY pads and “morph” should feel like instruments.
- **No surprises**: avoid parameter jumps; use pickup/soft takeover.
- **Performance safe**: rate limit, smoothing, and predictable timing.
- **Live friendly UI**: large controls, minimal text, dark mode, one-handed use.

### 2.2 Non-goals (for MVP)
- Full patch librarian via SysEx (S-1 does not support SysEx).
- Editing S-1 internal step data directly (not supported by MIDI CC).
- Audio input analysis (envelope follower) in MVP.

---

## 3) Target Users + Use Cases
### 3.1 Primary user
- Musician using S-1 for bass/lead/pads/chords/FX.
- Wants fast access to “sweet spots” and expressive control from a phone.

### 3.2 Key scenarios
- **Live jam**: one-handed macro + XY sweeps, morph between states for builds/drops.
- **Sound design session**: explore quickly, store bookmarks, iterate with modulations.
- **Composition**: build patterns in an easier sequencer, automate CC over time.

---

## 4) Functional Requirements

### 4.1 Core MIDI layer (foundation)
**FR-1: MIDI device selection**
- User selects S-1 output port (USB-MIDI or interface).
- User selects synth MIDI channel.
- Provide a “Test Note” button and “Panic” (All Notes Off).

**FR-2: Robust message sending**
- CC send, Note On/Off, Pitch Bend, Program Change, Clock/Start/Stop (optional).
- Throttle outbound CC bursts (see performance requirements).

**FR-3: State model**
- App maintains an internal state of all “owned” CCs (0–127 values).
- “Owned” params are those controlled by app macros/modulators/morph.
- App does not assume it can read back S-1 state (no SysEx).

**FR-4: Soft takeover / pickup**
- For any continuous UI control: do not jump S-1 value abruptly.
- Offer:
  - **Pickup mode**: control only activates after user crosses stored value.
  - **Relative mode** (optional): gesture sends incremental deltas.

---

### 4.2 Sweet Spot Macros (music-first control)
**FR-5: Macro controls**
- Provide 6–10 macros, each mapping to multiple CC destinations with curves and safe ranges.
- Each macro:
  - has a main value (0–100)
  - has adjustable min/max “range” per destination
  - optionally has a curve per destination (linear/log/s-curve)

**Default macro set (proposed)**
1) **Brightness** (cutoff + keyfollow + resonance compensation)
2) **Acid/Bite** (resonance + env depth + decay shaping)
3) **Punch** (amp ADSR emphasis)
4) **Motion** (LFO rate + depth + sync options)
5) **Animate** (PWM depth/source + osc LFO)
6) **Space** (delay + reverb macro)
7) **Grit/Noise** (noise level + optional filter shaping)
8) **Chord Spread** (chord voice shifts + poly mode usage)

**FR-6: Macro “lock”**
- User can lock a destination within a macro so it won’t be changed by the macro sweep.

---

### 4.3 Bookmarks + Morph (Polybrute-style)
**FR-7: Bookmarks**
- User can capture a “bookmark” which is a snapshot of app-owned CC values.
- Bookmarks stored locally (localStorage/IndexedDB) with name + tag + timestamp.

**FR-8: Morph between bookmarks**
- Select two bookmarks (A and B).
- Morph control:
  - slider (0→1)
  - optional XY morph (X = A↔B, Y = “wildness” or “space”)
- Interpolation rules:
  - Continuous CCs interpolate smoothly.
  - Discrete toggles either:
    - “snap at midpoint”
    - or “do not change” (safe mode)
- Send strategy:
  - only send CCs that differ between current state and target
  - batch updates with throttling

**FR-9: “Performance scenes”**
- A set of 8–16 bookmarks mapped to a grid for quick recall.

---

### 4.4 Modulation Engine (independent per-target LFOs)
**FR-10: Multi-modulator architecture**
- A “Modulator” is a generator producing values 0–127 over time.
- Each modulator can target one or more CC destinations with:
  - depth
  - offset
  - smoothing
  - min/max clamp

**FR-11: LFO module**
- Shapes: sine, triangle, ramp up, ramp down, square, random, sample&hold.
- Rate: free (Hz) or synced (musical divisions).
- Phase: free-run or reset rules:
  - reset on bar
  - reset on note-on
  - reset on transport start
- Swing / groove offset when synced (optional in v2).

**FR-12: One-shot ramp**
- Configurable rise/fall envelope for builds/drops (filter sweep, reverb swell, etc).
- Triggered by button, MIDI note, or sequencer event.

**FR-13: Modulation routing UI**
- “Add modulation” flow:
  1) pick destination CC
  2) pick mod source (LFO, one-shot)
  3) set depth/offset/rate/sync
- Include “mute mod” and “lock destination” controls.

---

### 4.5 XY Pads + Gestures (expressive control surface)
**FR-14: XY pad**
- Large pad with assignable X and Y destinations (CCs).
- Presets:
  - cutoff/resonance
  - cutoff/env depth
  - LFO rate/depth
  - delay time/level
  - reverb level/time
- Modes:
  - hold position
  - spring to center
  - inertia (ballistic glide)
  - quantized grid (optional)

**FR-15: Tilt control (optional)**
- Use device orientation as a mod source (if stable in Chrome).
- Provide calibration + deadzone + sensitivity.
- Must be easy to disable (performance safety).

---

### 4.6 Smart Sequencer (modern UI + voice-aware “smarts”)
**Positioning**
The app sequencer drives the S-1 by sending MIDI notes and CC automation lanes.
It does not edit the S-1 internal sequencer data.

**FR-16: Sequencer v1 (MVP)**
- 1 pattern, 1 track (expand later).
- Step grid (16/32/64 steps) with:
  - note value (or note lane)
  - gate length
  - velocity
  - probability
  - ratchet (basic)
  - tie/slide (implemented as longer gate or portamento behaviors)
- Transport:
  - internal clock or follow external MIDI clock
  - start/stop

**FR-17: CC automation lanes**
- Add lanes for key CCs (cutoff, env depth, LFO depth, FX sends).
- Per-step values with optional interpolation.

**FR-18: Voice-aware mode switching (“smart poly”)**
The sequencer can automatically change S-1 voice modes (via CC) based on the musical context:
- **Auto-unison on accents**
  - If velocity > threshold or step marked “accent”
  - temporarily switch Poly Mode to Unison for that note, then revert
- **Auto-poly for chords**
  - If chord detected (multi-note step) use Poly mode
- **Chord-mode engine (one-note chords)**
  - For chord steps, optionally:
    - set chord intervals (voice shifts) once
    - send only the root note per step
  - Useful for stable voicing and low MIDI traffic
- **Voice budget protection**
  - If overlapping notes would cause voice stealing, apply:
    - gate shortening
    - note priority rules
    - optional “mono lead” override

**FR-19: Pattern chaining (v2)**
- multiple patterns, chain order, song mode.
- per-pattern bookmark recall (sync bookmarks with patterns).

---

## 5) Information Architecture (Screens)
### 5.1 MVP navigation
- **Connect**
- **Perform**
  - Sweet Spot Macros
  - XY Pad
  - Bookmark grid + Morph
- **Mods**
  - LFO list + routing
- **Seq**
  - Step sequencer + automation lanes
- **Library**
  - Saved performances (bookmarks, macros, mod setups)
- **Settings**
  - MIDI channel, rate limits, pickup mode, clock source, UI preferences

---

## 6) Data Model (Implementation Guidance)
### 6.1 Core objects
- **CCState**
  - { ccNumber: int, value: 0–127, owned: bool }
- **Macro**
  - name, knobValue (0–1)
  - destinations: [{cc, min, max, curve, lock}]
- **Bookmark**
  - id, name, tags
  - ccSnapshot: {ccNumber: value}
- **Morph**
  - sourceBookmarkId, targetBookmarkId, amount (0–1)
  - discretePolicy: snap / ignore
- **Modulator**
  - id, type (LFO/oneshot)
  - params (shape, rate, sync, phase, depth, offset, smoothing)
  - targets: [{cc, depth, offset, clampMin, clampMax}]
- **SequencerPattern**
  - steps: [{note(s), velocity, gate, prob, ratchet, ties}]
  - automationLanes: {ccNumber: [stepValues...]}

### 6.2 Ownership + conflict resolution
- App “owns” a CC when:
  - it is controlled by a macro, morph, modulator, or sequencer lane
- If multiple sources control same CC:
  - define a merge policy:
    - base = bookmark/macro value
    - add modulation as delta
    - clamp to [0,127]
    - sequencer automation can override base for that step window (optional)
- Provide a visible “ownership” indicator in UI.

---

## 7) Performance Requirements (Must-haves)
**PR-1: Rate limiting**
- Per-destination CC updates should be capped (e.g., 30–60 Hz).
- Morph sends should be batched and throttled.

**PR-2: Smoothing**
- Optional smoothing (slew limiter) for fast gestures to avoid zipper noise.

**PR-3: Low latency feel**
- Gesture-to-sound should feel immediate.
- Avoid large GC pauses; prefer requestAnimationFrame scheduling for UI + timers for MIDI.

**PR-4: Offline + fast startup**
- PWA should load instantly after install.
- No network required for core operation.

**PR-5: Safe transport**
- If app drives clock, it must be stable and resilient to backgrounding.
- If device is backgrounded, app should warn about timing risk.

---

## 8) UX Requirements
- Thumb-friendly controls
- Dark mode default
- “Live mode” (minimal UI chrome)
- Large buttons for:
  - Panic
  - Capture bookmark
  - Morph
  - Mod mute
- Haptics (optional) for discrete toggle confirmation

---

## 9) QA / Test Plan
- Confirm:
  - device connect/disconnect resilience
  - CC correctness across pages
  - no parameter jumps with pickup enabled
  - morph correctness for continuous vs discrete
  - modulator stability over 10+ minutes
  - sequencer timing under:
    - internal clock
    - MIDI clock follow
  - voice-aware switching does not glitch (rate limited and scheduled)

---

## 10) Milestones (Suggested)
### MVP (usable in a week or two of focused work)
1) Connect screen + basic CC send
2) Sweet Spot Macros (6 macros)
3) Bookmarks A/B + Morph slider
4) XY pad with presets
5) Two LFO modulators (independent)
6) Sequencer v1 (notes + gate + velocity + probability)

### v2 (turn it into “the S-1 expansion pack”)
- Scene grid (8–16 bookmarks)
- Morph XY
- Modulation matrix UI polish
- Sequencer automation lanes
- Smart poly/unison/chord rules
- Pattern chaining + per-pattern scenes
- Import/export performances as JSON

---

## 11) Success Criteria
- User can do a full sound design session without touching S-1 menus.
- User can perform transitions (build/drop) with morph and XY gestures.
- User can create movement using multiple independent LFOs.
- Sequencer is faster to use than on-device step editing and “feels modern.”
- The app reliably performs without MIDI flooding or value jumps.

---

## 12) Open Questions (for later, not blockers)
- Preferred sequencer style:
  - Elektron-ish parameter locks vs Ableton clip style vs hybrid
- How aggressive should voice-aware switching be (toggle vs subtle)?
- Should macros be “genre presets” (bass/lead/pad templates)?
- Should the app store multiple “device profiles” for future AIRA compacts?

