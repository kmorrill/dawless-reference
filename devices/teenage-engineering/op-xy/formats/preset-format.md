# OP-XY preset format and sample-based instrument design

Summary of preset structure and practices for building drum kits, tonal samplers, and multisample instruments.

## 1) Preset basics
- Presets are folders ending with `.preset` containing `patch.json` and WAVs. Top-level fields include engine, envelope, fx, lfo, preset_settings, regions, type, version (see gist spec).
- `regions` entries reference WAVs and define start/end (0-1), lokey/hikey, pitch_keycenter, tune, play_mode, loop_mode, direction, pan, gain, mute_group, etc.

## 2) Sample rate and quality
- Supports 44.1 kHz or 48 kHz, 16- or 24-bit. Large files reduce polyphony/load speed.
- Community guidance: down-sample to ~22 kHz when possible; keep long samples under ~20 s.

## 3) Drum map and MIDI notes
- Drum sampler uses fixed MIDI notes (spec/opxyloop map): kick 53, snare 55, clap 58, closed hat 61, open hat 62, low tom 65, crash 66, etc.
- Map lokey/hikey/pitch_keycenter to these notes so pads trigger correctly; aliases like ch/oh are sometimes normalized.

## 4) Drum sampler presets
- One region per pad. Fields: sample, start/end, lokey/hikey (same note), pitch_keycenter, play_mode (0 gate, 1 one-shot, 2 note on/off), loop_mode (0 none, 1 forward, 2 ping-pong), direction (0 fwd, 1 rev), pan, gain; optional tune, mute_group.
- Practices: trim tightly, normalize, consider round-robin/velocity layers via filename suffixes, use standard names for auto-mapping, often down-sample to 11/22 kHz for drums.
- Tools: opxy drum builder (web), buba447 opxy-drum-tool, teopxy (Python), OP-PatchStudio (PWA).

## 5) Tonal sampler presets
- Single-sample approach: set lokey=0, hikey=127, pitch_keycenter to sample's root; play_mode 0 for gate or 1 for one-shot; enable loop_mode=1 with loop points plus fade_in/fade_out for sustain.
- Best practices: accurate keycenter, restrict range if stretching sounds bad, choose loop points at zero crossings, small crossfades to avoid clicks.
- Tools: buba447 multisample tool (single-sample mode), opxy-multisampler builder, OP-PatchStudio.

## 6) Multisample presets
- Multiple zones with note ranges; example fields include loop_start/loop_end, fade_in/fade_out, gain, etc. (see gist spec).
- Practices: name files with root notes (C4 or MIDI numbers), set lokey/hikey per zone (overlap slightly), pitch_keycenter to root, loop_mode=1 for sustaining sounds, use crossfades, keep samples <= ~20 s and total zones <= 24, velocity/round-robin via suffixes (_v1/_rr1, etc.).
- Tools: opxy-multisampler builder, buba447 multisample tool, OP-PatchStudio.

## 7) Workflow summary
- Choose instrument type (drum, tonal, multisample).
- Prepare audio: trim, normalize, tune; down-sample when appropriate.
- Map samples: drums to fixed notes; tonal/multisample set ranges and pitch_keycenter.
- Edit loops/crossfades; set play_mode and loop_mode per use.
- Create `patch.json` to match spec; ensure filenames match regions; install by copying `.preset` folder with WAVs to device storage.

## 8) Key takeaways
- Understand `patch.json` fields (regions, play/loop modes, ranges) from the spec.
- Use community tools for mapping, sample-rate conversion, loop editing, velocity/round-robin.
- Follow best practices: trimmed samples, sensible sample rates (11/22 kHz for drums; 44 kHz for hi-fi), clear naming, correct note mapping.
- Respect drum map for kits and keep multisample limits (<= 24 zones, <= ~20 s per sample).
