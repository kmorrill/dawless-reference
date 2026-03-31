# OP-XY Guide Notes (from 673624c81db2feb0839f6e76_original.pdf)

## Index overview
- Sections include hardware overview, layout, main modes (instrument, auxiliary, arrange, mix), tracks, sequencer, transport, keyboard, sample, projects, tempo, com, players, bar, volume, encoders, step components, players (arpeggio/maestro/hold), project management.

## Hardware highlights
- 64-step sequencer, 24-voice polyphony, 1920 PPQN, parameter locks, step components, stereo signal path, MIDI over BLE, 16 programmable tracks, built-in effects, drum/synth sampler, USB audio/MIDI host & device.

## Layout essentials
- 8 instrument tracks + 8 auxiliary tracks; track buttons 1-8 select tracks; active track light white (instrument) or red (aux).
- Sequencer row runs across middle; heart of device.
- Transport controls bottom-left (record/play/stop/octaves/shift).
- Keyboard: two octaves below transport; accidentals vs naturals (colorless caps).
- Sample button top-right: sample from any input incl. built-in mic.
- Projects button: create/edit projects.
- Tempo button near projects: BPM, swing, metronome.
- Com button: system settings, connections (wired/wireless), outputs.
- Players button: note effects (arpeggio/chords/etc.).
- Bar button: extend sequence, quantization params.
- Volume: rotary top-left near speaker.
- Encoders: 4 above track buttons (dark→light gray), adjust parameters.

## Mode model
- Main modes: instrument (compose), auxiliary (transpose, sends, externals, punch-in FX), arrange (chain patterns into scenes), mix (levels/pan per track, master EQ/compressor).
- Each mode except arrange has 4 modules (M1–M4) accessed via screen-bottom buttons; extra params sometimes via shift.

## Tracks and linking
- Track buttons under encoders select instrument/aux tracks; presets selectable via hold+track.
- Link up to 4 tracks: hold one track, press others to link; primary controls linked while in linked tracks.

## Sequencer basics
- 64-step grid across device used to record notes/sounds; central to pattern editing.

## Project defaults
- New project: hold M1 in project view. Default instruments: 1–2 drums, 3 bass, 4 pluck, 5 lead, 6 soft pluck, 7 strings, 8 pad.

## Guide conventions
- Single/combo/sequence press, hold, rotate/click encoder, hold+rotate; diagrams for keyboard/chord context.

## Relevance for preset generator
- Need to map stems to 8 instrument/8 aux tracks; presets per track selected via hold+track.
- Patterns are 64-step; bars extendable via bar button—export should align slices to bars/steps.
- Arrange mode chains patterns into scenes; useful if presets encode pattern scenes.
- FX/punch-in and sends live in auxiliary mode modules; keep unknown fields intact in template JSON.
- Tempo/swing/metronome configurable per project; may need to match stem BPM.
- Players (arpeggio/chord) can affect playback of sequences; preserve related preset fields.
- Master mix includes EQ/compressor; do not drop those fields if present in template.
