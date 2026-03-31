# Scenes and Songs

## Scope
This document captures stable format findings for scene/song state from
`unnamed 149/150/151/152/154/155`, follow-up `b/nl/lp` probes, and
`bleez` scene-edit chains (`bleez`, `bleez1`, `bleez2`, `bleez7`-`bleez12`).

## Where Scene/Song Data Lives
Scene/song state is not isolated to a fixed header field.
Current stable model is split storage:

1. Pre-track control bytes/records (before Track 1).
2. Track 16 control bytes (tail control region in normalized branch).

## Stable Findings

### 1) Loop Is Per-Song (Normalized Branch)
Loop toggles were isolated as Track 16 control-byte changes:

- Song 1 (`150 nl` <-> `150 lp`):
  - `track16+0x0169/+0x016A`: `01 00` (off) <-> `00 01` (on)
- Song 2 (`154 loop` <-> `154 nl`):
  - `track16+0x016E`: `00` (on) <-> `01` (off)
- Song 3 (`151 nl` <-> `151 lp`):
  - `track16+0x0171/+0x0172`: `00 01` (on) <-> `01 00` (off)

Note: Off/on polarity above follows user-confirmed capture intent labels.

### 2) Scene Count/List Uses Track 16 Control Bytes
For Song 2 arrangement captures:

- `154` (Song2 + Scene2) includes `track16+0x0163 = 0x02` with a short
  structural payload.
- `155` (Song2 with 3 arranged scenes) includes `track16+0x0163 = 0x03` with a
  larger structural payload.

This strongly indicates scene-count/list control in Track 16 tail bytes.

### 3) Scene Mute State Is Persisted
Mute probes (`150b/152b/154b/155b`) show:

- Variable-length pre-track record insertions.
- Coordinated Track `9..16` normalized-branch rewrites.

So mute state is serialized (not transient UI state).

### 4) Song Chaining Requires Coordinated Pre-Track + Track16 State
On a `j06`-family 9-pattern arrangement (`pre_track_len=131`), controlled
Time After Time probes produced:

- `01_time-after-time_song_arranged_s2s3_track16_only.xy`
  - Track16 patched with `song2_s2s3_struct_155` (`track16+0x15F` starts `0x03`)
  - **Loads**, but no visible song chaining.
- `02_time-after-time_song_arranged_s2s3_with_pretrack.xy`
  - Same Track16 as above, plus pre-track bytes `pre[0x0F:0x12] = 02 01 00`
  - **Crashes**.
- `03_time-after-time_song_arranged_s2_with_pretrack.xy`
  - `song2_scene2_struct_154` on Track16 (`track16+0x15F` starts `0x02`)
  - pre-track bytes `pre[0x0F:0x12] = 01 01 00`
  - **Loads**, and Song 2 chains Scene 1 -> Scene 2.
- `08_time-after-time_song_arranged_t16s2_only.xy`
  - Same `song2_scene2_struct_154` Track16 structure with baseline pre-track
    (`pre[0x0F:0x12] = 00 00 10`)
  - **Loads**, and Song 2 still chains Scene 1 -> Scene 2.
- `04_time-after-time_song_arranged_s2_pre_t16count3.xy`
  - From the working `03` branch, only `track16+0x15F` bumped `0x02 -> 0x03`
    (count-only probe)
  - **Loads**, Song 2 shows Scene sequence `1,2,2`.
- `05_time-after-time_song_arranged_s2_pre_t16s2s3.xy`
  - pre-track `01 01 00` + full `song2_s2s3_struct_155` Track16 payload
  - **Loads**, Song 2 shows Scene sequence `1,2,3`.
- `06_time-after-time_song_arranged_pre020100_t16s2.xy`
  - pre-track bytes `pre[0x0F:0x12] = 02 01 00` with 154-style Track16
  - **Crashes** (`num_patterns > 0`).

Implication:
- For 2-scene Song2 chains, 154-style Track16 structure appears sufficient;
  pre-track mutation is not strictly required (`08` passes).
- Count byte at `track16+0x15F` affects sequence length; without additional
  scene-id structure it repeats prior scene (`04` => `1,2,2`).
- Full 155-style Track16 structure carries additional scene-id information
  needed for `1,2,3` (`05`).
- `pre[0x0F:0x12] = 02 01 00` is crash-prone in this branch (`02`, `06`),
  while `01 01 00` is compatible with both 154/155 Track16 paths.

### 5) Provisional: Scene Pattern-Map Edits Can Serialize as Pre-Track Inserts
From user-provided chain `src/bleez.xy -> src/bleez1.xy -> src/bleez2.xy`:

- `bleez -> bleez1` (first override: Scene2/Track3->P1) shows branch entry:
  - pre-track `replace @ +0x10:0x12`: `01 00 -> 11`
  - pre-track `insert @ +0x5D` (11 bytes):
    `08 08 00 00 08 08 03 00 00 16 01`
  - Track16 rewrite at `+0x11F`: `19 -> 11 40 00 00 01 40 00 00 01` (net `+8`)
  - broad track-body rewrites (normalized branch transition)

- `bleez1 -> bleez2` (second override: Scene3/Track4->P1) is additive:

- `pre[0x0F]`: `0x01 -> 0x02`
- pre-track insert at `+0x67` (13 bytes):
  - `08 08 00 00 00 00 08 08 02 00 00 16 01`
- Track16 control window unchanged (`ff 00 00 9b 01 00 00`)
- No track body bytes changed (all note/event payloads identical).

Interpretation (confidence: medium):
- At least one class of per-scene pattern-selection edits is serialized via
  variable-length pre-track records, not by rewriting track event bodies.
- First override in this branch may trigger normalization rewrites; subsequent
  overrides can appear as pre-track record appends with stable Track16.
- Exact token schema for `{scene, track, pattern}` remains unresolved.
- Counterexample: matching `pre[0x0F:0x12]` + matching 155-style Track16
  payload is not always sufficient to reproduce device-visible scene behavior
  on generated files, so additional branch-consistency constraints exist.

### 6) Provisional: `bleez7`-`bleez12` Narrowed Candidate Fields
From `bleez` scene probes (`bleez7`-`bleez12`):

- `bleez1` and `bleez9` are byte-identical.
- Across `bleez1 -> bleez{7,8,10,11,12}`, Track16/P1 body is unchanged; edits
  serialize in pre-track variable records only.
- `bleez1 -> bleez11` is a single-byte pre-track change (`... 00 00 ...` ->
  `... 00 01 ...`) with no other structural deltas; this is a strong candidate
  for a pattern-index field (`P1 -> P2` class).
- `bleez1 -> bleez7` changes `... 00 00 ...` -> `... 01 00 ...` plus a paired
  byte (`0x03 -> 0x02`) in the same token; this suggests a track selector
  field plus a coupled record field (meaning still unresolved).
- `bleez1 -> bleez10` expands an 11-byte record form to a 13-byte form:
  - `08 08 aa bb 08 08 cc 00 00 16 01` ->
  - `08 08 aa bb 00 00 08 08 cc 00 00 16 01`
- `bleez12` (intended revert) does not return to base pre-track; it compacts to
  a shorter variant that duplicates baseline token `08 08 06 00 00 16 01`.

Implication:
- We now have stronger evidence for compact per-scene record fields in
  pre-track (track/pattern candidates), but cannot yet decode full
  `{scene,track,pattern}` schema or `R11` vs `R13` record semantics.

### 7) Device-Validated Constraint: `R13` `bb` Byte Is Crash-Sensitive
From scene contribution probes `31`-`36` (device outcomes on 2026-03-01):

- `31` (`R11 bb:00->02`) -> pass
- `36` (`R11 bb:00->01`) -> pass
- `34` (`R13 bb:00->01`) -> crash
- `35` (`R13 aa:00->01`, `cc:02->01`) -> pass

Inference (medium-high confidence):
- `R11` and `R13` are structurally different token forms with different valid
  value domains.
- In tested `bleez2` context, `R13 bb` appears constrained (likely reserved or
  coupled), and mutating it alone is unsafe.
- This weakens the earlier assumption that `bb` is universally the pattern
  field across all scene-record forms.

Authoring implication:
- Do not mutate `R13 bb` directly in generated files until schema closure.
- Prefer `R11`-form edits for pattern-index probing.

### 8) Provisional: Device-Authored `00`-`07` Chain Confirms Split Model In Fresh Project
From `00_scene_seed` -> `07_scene_s3_t4p3` (device exports, 2026-03-01):

- `01 -> 03` modifies only Track16/P1 for song scene-list growth:
  - `01` (`... +0x167=0x01 ...`) -> Scene1 baseline
  - `02` (`... +0x167=0x02 ...`) + inserted list bytes -> Scene1,2
  - `03` (`... +0x167=0x03 ...`) + one more list byte -> Scene1,2,3
- `03 -> 07` keeps Track16 fixed while scene pattern overrides serialize in
  pre-track records:
  - `04` adds `... 00 01 00 00 1b 01 00 00 ...` (Scene2/T3 override class)
  - `05` adds T4-class record bytes around `... 1a 01 00 00 ...`
  - `06` reverts to byte-identical `04` (exact rollback)
  - `07` bumps `pre[0x0F]: 0x01 -> 0x02` and adds
    `... 00 01 02 00 00 1a 01 00 00 ...` (Scene3/T4 override class)
- Track-event payloads on edited tracks remained unchanged in `03 -> 07`; edits
  were structural pre-track record updates.

Interpretation (confidence: medium-high):
- This independently reproduces the split-storage scene model in a fresh
  user-authored branch:
  - Track16 controls song scene-list structure.
  - Pre-track variable records carry per-scene pattern-map deltas.
- Candidate track tags (`0x1b` for T3-class, `0x1a` for T4-class) strengthen
  the `{scene,track,pattern}` field-candidate model, but full schema remains
  unresolved.
- New byte-correlation: inserted tails in this chain align with known
  descriptor bodies (`...0100001b010000`, `...010100001a010000`,
  `...010200001a010000`), suggesting scene records may embed descriptor-like
  track/pattern subfields with additional wrapper bytes.
- `04 -> 05` (same-scene second override) uses in-token growth/rewrite rather
  than a simple append, analogous to `bleez` expanded-form behavior (`R11` ->
  `R13` class), further supporting packed same-scene forms.

### 9) Provisional: Cross-Branch Anchor Shift + Track16 Identity Constraints
From cross-branch comparison of `unnamed 150/154/155`, `00`-`07`, `bleez*`,
and Time-After-Time/sceneprobe files:

- Non-normalized branch scene-list structural ops anchor near Track16
  `+0x15F`.
- Normalized branch scene-list ops use the same structural script but shifted
  to around `+0x167`, with fixed prefix at `+0x15F`:
  - `ff 00 00 9b 01 00 00`
- In normalized branch, many scene-different files share identical Track16/P1
  body bytes (same hash), while behavior differences are encoded in pre-track
  records.

Implication (confidence: medium-high):
- Track16 scene-list bytes are branch-relative and can be constant across
  files with different scene override behavior in normalized families.
- Per-scene map decode/writer logic must model branch anchor shifts and
  pre-track record deltas together.

### 10) Byte-Level Negative Control: Generated Sceneprobe Variants
Generated sceneprobe sets (`03`-`06`, `21`-`24`) intended to differ in scene
pattern mapping were found to have identical scene-relevant pre-track and
Track16 bytes within each set.

Implication (confidence: high):
- Their intended scene-map differences were not serialized in currently patched
  scene byte regions.
- Use these as negative controls for scene-map serialization coverage, not as
  positive evidence that per-scene map writes are encoded.

### 11) Follow-up `08`-`10`: Scene3 `T4` Pattern Variant Captured
From `08_scene_s2_t4p2_only`, `09_scene_s3_t4p2_from06`,
`10_scene_s3_t4p3_from06`:

- `08` is byte-identical to `05_scene_s2_t4p2` (no new serialized form).
- `10` is byte-identical to `07_scene_s3_t4p3` (reproducibility control).
- `09` is a new Scene3/T4 pattern variant with pre-track insert:
  - `... 00 01 01 00 00 00 1a 01 00 00 ...` (P2-class in this context)
- `10`/`07` Scene3/T4 P3-class remains:
  - `... 00 01 02 00 00 1a 01 00 00 ...`
- `09 -> 10` differs by local normalization (`01 00 -> 02`) with net `-1`
  byte, Track16 unchanged.

Implication (confidence: medium-high):
- We now have same-scene/same-track `P2` vs `P3` serialized forms in the
  normalized branch without Track16 mutation.
- This supports compact/variable-length pattern-field encoding inside
  pre-track scene records and reinforces that `pre[0x0F]` acts as scene-target
  context in this family.

### 12) Follow-up `11`-`13`: Reversible Packed Variant (Scene2/T4 family)
From `11_scene_s2_t4p2`, `12_scene_s2_t3p1`, `13_scene_s2_t3p2`:

- `11` and `13` are byte-identical (same SHA1); `12` is the only unique file.
- `11 -> 12` is a pre-track-only `-2` byte compaction:
  - deletes at `@0x5c` and `@0x5f` (both `00`) in the scene-record band.
- `12 -> 13` is the exact inverse (`+2` byte insertions), restoring `11`.
- `11 -> 13`: no bytes changed.
- `11/12/13` all keep Track16 scene-list unchanged (`count=3`, IDs `[0,1,2]`)
  and show no logical-entry deltas in corpus compare.
- `05/08 -> 11` differs only by `pre-track` control-byte expansion at `@0x10`
  (`11 -> 01 00`), with no logical-entry changes.
- Device-verified behavior (Scene2):
  - `11`: `T3=P2`, `T4=P2`
  - `12`: `T3=P1`, `T4=P2`
  - `13`: `T3=P2`, `T4=P2`

Implication (confidence: high):
- The `11 -> 12` two-byte pre-track deletion is behavior-carrying: it removes
  the `T3` override while retaining `T4->P2` in Scene2.
- `11/13` represent the packed dual-override endpoint in this branch family.
- Hash/diff validation remains required because filename intent alone is not a
  reliable uniqueness signal.

### 13) Hypothesis Pack `41`-`50`: Crash Boundary for Naive Track Generalization
From generated staged probes in `output/scene-probes/41..50`:

- `41` (`Scene2 T4->P2` control) passes with expected behavior.
- `42` (`Scene2 T4->P3`) passes with expected behavior.
- `43..46` (descriptor-tail style extrapolation to `T5..T8`) all crash with
  `fixed_vector.h:59 i < length`.
- `47..50` (two schema guesses each for `T2`/`T1`) all crash with the same
  assertion.
- Across the pack, Track16 was held constant and only pre-track scene-region
  bytes were mutated.

Implication (confidence: high):
- In this normalized Scene2 override family, the currently valid mapping is
  confirmed for `T4` pattern changes, but direct descriptor-like extrapolation
  to `T5..T8` and naive `T1/T2` encodings is invalid/crash-prone.
- Additional wrapper/branch constraints are required before generalizing scene
  override writing to all tracks.

### 14) Hypothesis Pack `51`-`58`: Pre-Track Insert Boundary on All-Tracks Base
From generated probes on `03_time-after-time_song_arranged_s2_with_pretrack`
(`pre_track_len=131`, `T1..T8` all at 9 patterns):

- Tested files `52..58` all crash with `num patterns` assertion.
- This includes two "known-family" `T4` controls (`52`, `53`) plus `T5`
  descriptor-tail variants (`54`, `55`), a two-record packed variant (`56`),
  and `bleez`-style `R11` inserts (`57`, `58`).
- Across the pack, Track16 and `pre[0x0F:0x12]` were held constant; only
  pre-track insertion bytes near the sentinel/handle boundary changed.

Implication (confidence: high):
- In this `pre_track_len=131` branch family, scene payload insertion at that
  pre-track location is structurally invalid regardless of token style.
- The dominant failure is branch-invariant mismatch, not just wrong
  `{track,pattern}` values.
- Next decode progress should prioritize device-authored captures in this exact
  branch to discover the native mutation path instead of synthetic inserts.

### 15) Device-Tested Scene Probes: Pattern-Existence Constraint + Authoring Recipe
From generated probes `probe1`-`probe5` (device outcomes on 2026-03-01):

Five targeted byte mutations on corpus files `04`/`07`:

| Probe | Change | Target Track→Pattern | Track Has Pattern? | Result |
|-------|--------|---------------------|--------------------|--------|
| 1 | Tag swap T3→T4 | T4→P2 | T4 has P1 only | CRASH |
| 2 | Tag swap T4→T3 | T3→P3 | T3 has P1+P2 | CRASH |
| 3 | P3→P2 pattern swap | T4→P2 | T4 has P1+P2+P3 | PASS |
| 4 | Tag swap T3→T2 | T2→P2 | T2 has P1 only | CRASH |
| 5 | New Scene 4 + T3→P2 | T3→P2 | T3 has P1+P2 | PASS |

All three crashes: `num_patterns > 0` (`serialize_latest.cpp:90`).

Confirmed findings (confidence: high):

1. **Track tag formula**: `tag = 0x1E - track_1based`
   - `0x1B`=T3, `0x1A`=T4 proven by successful probes.
2. **Variable-length pattern encoding**: P2=`01 00` (2B), P3=`02` (1B).
   - Probe 3 swapped 9B P3 record to 10B P2 record; loads cleanly.
3. **Pattern-existence constraint**: Scene records MUST reference patterns that
   exist on the target track. Violation crashes with `num_patterns > 0`.
4. **Scene creation recipe** (Probe 5, all 4 regions coordinated):
   - `pre[0x0F]` ordinal increment
   - Record insertion before handle table
   - T1 preamble[0] decrement by `0x21`
   - T16 scene count increment + ID append
5. **T1 preamble formula**: `T1_preamble[0] = 0xD6 - (ordinal + 1) * 0x21`
   - Ordinal 0: `0xB5`, 1: `0x94`, 2: `0x73`, 3: `0x52`

Authoring implication:
- Scene override authoring is now viable for T3/T4 in the `00`-`07` corpus family.
- Must verify target track has the referenced pattern before emitting a scene record.
- Full recipe is: ordinal + record + T1 preamble + T16 list (all four required).

Detailed analysis: `docs/logs/2026-03-01_scene_probe_v1_results.md`

### 16) V2 Scene Probes: Record Forms, Deletion, and Sticky Overrides
From generated probes `v2-1` through `v2-5b` (device outcomes on 2026-03-01):

All six probes loaded successfully (zero crashes):

| Probe | Change | Result | Key Finding |
|-------|--------|--------|-------------|
| v2-1 | Compact 8B T4→P2 in S2 | PASS | Compact record form works |
| v2-2 | 10B dual-override T3+T4→P2 in S2 | PASS | Dual-override form works |
| v2-3 | 9B T4→P3 replacing S2's T3→P2 | PASS | T3 stayed on P2 (unexpected) |
| v2-4 | 5 scenes (add S5 T3→P2) | PASS | 5 scenes + song sequencing |
| v2-5a | Scene 3 deletion (full) | PASS | Deletion recipe confirmed |
| v2-5b | Scene 3 deletion (T16 unchanged) | PASS | Orphaned T16 entries tolerated |

Confirmed findings (confidence: high):

1. **All record forms interchangeable**: Compact 8B, standard 8B/9B, and dual 10B
   forms all work when swapped into different scene positions.
2. **Scene deletion works**: Creation recipe is fully reversible (remove record +
   decrement ordinal + increment T1 preamble + adjust T16).
3. **5 scenes confirmed**: Formula scales beyond 4 with same ordinal/preamble math.
4. **T16 is not authoritative**: Pre-track records are the source of truth for
   visible scene count. Orphaned T16 entries are silently ignored.
5. **Sticky override in v2-3 was a probe bug** (resolved by file 14 capture):
   Our v2-3 used `00 01 02 ...` as the first record (wrong: leading `00` is an
   inter-record separator, not part of the first record). The device writes
   `01 02 00 00 1a 01 00 00` (compact form, no `00` prefix) for the first record.

### 16b) File 14 Capture: Record Structure + T1 Preamble Correction
Device capture `14scene-s2t4p3.xy` (S2 T4→P3 edit from file 07) revealed:

1. **Record terminator is `[tag] 01 00 00`** where tag is a track tag byte.
   The tag byte disambiguates from `01 00 00` sequences that appear mid-record.
2. **Inter-record separator is `00`**: first record has no `00` prefix,
   subsequent records do. Compact first records start with `01 XX ...`.
3. **T1 preamble formula CORRECTED**: driven by record count, NOT `pre[0x0F]`:
   `T1_preamble[0] = 0xD6 - (record_count + 1) * 0x21`
   Verified against ALL 7 corpus files (0-3 records → 0xB5/0x94/0x73/0x52).
4. **`pre[0x0F]` ordinal ≠ record count**: file 14 has ordinal=1 but 2 records.

Detailed analysis: `docs/logs/2026-03-01_scene_probe_v2_results.md`

### 17) Corpus Re-Analysis with Corrected Parser

Re-analysis of ALL scene/bleez corpus files with the file-14-corrected parser
(anchor-based `[tag] 01 00 00` splitting) revealed:

**Bleez corpus uses different record encoding:**
- Scene corpus (files 00-14): Track tags `0x1A`-`0x1B` (T3/T4), records 8-10B,
  `00` inter-record separator.
- Bleez corpus (bleez1-bleez12): Track tag `0x16` (T8), `08 08` sub-record
  delimiters, `0x1F` inter-record separators. Records are significantly larger
  (44-111B) — likely encoding multi-track overrides with a richer structure.
- The parser's `[tag] 01 00 00` anchoring works for the scene corpus but bleez
  records don't use that terminator pattern.

**Ordinal (`pre[0x0F]`) meaning clarified:**
- NOT a record count. Tracks the number of **unique override configurations**.
- File 14: ordinal=1 but 2 records (both encode identical T4→P3).
- Bleez corpus: ordinal values up to 11 (bleez7, which has 12 scenes).
- In the scene corpus, ordinal typically equals `record_count` because each
  record is unique. File 14 is the exception proving the distinction.

**Universal T1 preamble formula confirmed (30/30 files):**
- `T1p = 0xD6 - n * 0x21` where `n` is determined by scene complexity.
- Scene corpus (15 files): n = record_count + 1. 15/15 match.
- Bleez corpus (15 files): n = (value derived from scene structure). 15/15 match.
- The formula `0xD6 - n * 0x21` is universal across BOTH encoding families.

### 18) Hybrid `bleez35` Branch: Single-Byte `R11` Selector (Device-Validated)
From probes `66`-`71` on `bleez35`-family files (`pre_track_len=154`,
matrix-format region with mixed `7B/11B/13B` records):

- `66` baseline behavior (user-validated):
  - Scene1: all tracks `P9`
  - Scene2: all tracks `P9` except `T3=P1`
  - Scene3: `T4=P1`, `T5=P1`, others `P9`
- `67` changed one byte (`pre@0x5F`, `rec2[3]: 00->01`) and produced exactly
  one behavior change: Scene2 `T3: P1->P2`.
- `68` (`rec2[3]=02`) and `69` (`rec2[3]=03`) both pass and step Scene2/T3 to
  `P3` and `P4` respectively (user-validated).
- Boundary controls `70`/`71` (touching adjacent bytes) crash:
  - `70` (`rec2[2]` + selector edit) -> `num_patterns`
  - `71` (`rec2[4]` + selector edit) -> `fixed_vector`

Inference (confidence: high for this branch family):
- `rec2[3]` is a stable Scene2/T3 pattern selector lane:
  - `00->P1`, `01->P2`, `02->P3`, `03->P4`
- Immediate neighbors around this lane are crash-sensitive and should be
  treated as coupled/guarded fields.

### 19) Hybrid `bleez35` Follow-up `72`-`77`: `P5..P8` Confirmed, `P9` Boundary
From device-tested follow-ups after the `66`-`71` selector work:

- `72`..`75` pass and continue the same selector walk:
  - `rec2[3]=04` -> Scene2/T3=`P5`
  - `rec2[3]=05` -> Scene2/T3=`P6`
  - `rec2[3]=06` -> Scene2/T3=`P7`
  - `rec2[3]=07` -> Scene2/T3=`P8`
- `76` (`rec2[3]=08`) crashes with `num_patterns`.
- `77` pass confirms one-byte non-`T3` change in the decoded matrix lane:
  - `pre@0x6E: 08->00` yields Scene3/T7 `P9->P1` with other tracks unchanged.

Inference (confidence: high for this branch):
- `rec2[3]` valid value domain is currently `0x00..0x07` (`P1..P8`).
- `P9` likely is not represented as explicit `0x08` in this `R11` lane; it is
  likely represented by default/no-override state instead.
- Non-`T3` per-track edits are now device-confirmed via the matrix-vector
  record path (at least for Scene3/T7 in this family).

### 20) Follow-up `78`-`87`: No-Override Fails, T7 Sweep Mostly Works
Device-tested follow-ups after section 19:

- `78`/`79` (remove full `rec2` record, with/without `pre[0x0F]` decrement)
  both crash (new no-assert presentation on device).
- `80` (`pre@0x6E:08->01`, intended Scene3/T7=`P2`) crashes with
  `num_patterns`.
- `81`..`87` pass and set Scene3/T7 as intended:
  - `0x02..0x08` -> `P3..P9`
  - plus earlier `77` result: `0x00` -> `P1`

Inference (confidence: medium-high):
- `R11 rec2` no-override by record deletion is not currently a safe write path
  for this branch family.
- Scene3/T7 matrix-lane values are accepted for `{0x00, 0x02..0x08}` but
  rejected at `0x01`.
- This strongly suggests `P2` in this lane likely requires an alternate
  serialized form (likely long-form `01 00`) instead of single-byte `0x01`.

### 21) Follow-up `88`-`92`: Long-Form Fails, Preamble-Coupled Drop Loads
Device outcomes:

- `88`/`89`/`90` (long-form rec3 variants) all crash with `num_patterns`.
- `91`/`92` (rec2 drop + `T1 preamble[0]=0x94`) both load.
- `91`/`92` behavior split with one-byte control delta:
  - `91` (`pre[0x0F]=0x02`) shows 3 scenes
  - `92` (`pre[0x0F]=0x01`) shows 2 scenes
  - shared behavior:
    - Scene1: all tracks `P9`
    - Scene2: `T4/T5=P1`, other tracks `P9`
  - `91` Scene3: all tracks `P1`

Inference:
- The `T7=P2` failure is not solved by simple long-form width expansion alone;
  there are additional constraints in this matrix lane.
- Prior rec2-drop crashes were at least partly due to missing coupling with
  `T1 preamble`; correcting `0x73 -> 0x94` restores loadability.
- `91` and `92` differ only by `pre[0x0F]` (`0x02` vs `0x01`); behavioral
  meaning is now confirmed in this branch as scene-slot count/topology control
  (`0x01->2 scenes`, `0x02->3 scenes`) with stable scene-map payload bytes.

### 22) Manual Resave Probe: Selection-Only Saves Mutate Structure
From manual resave set (2026-03-03):
- Song select only (`02`/`03` from `unnamed150` seed)
- Scene select only (`04`/`05` from `91` seed)
- Track/pattern focus only (`07`/`08` and `09`/`10`)

Observed invariants:

1. Song selection mutates pre-track control byte and rewrites Track `9..16`.
- `pre[0x0F:0x12]`: `00 00 10 -> 00 00 01` (Song2) and `-> 00 00 02` (Song3)
- File size `+64`; Track `9..16` bodies each `+8`.

2. Scene selection mutates scene-slot control and can trigger broad
   normalization despite stable decoded matrix payload.
- `04` (select Scene1): `pre[0x0F:0x12] 02 11 00 -> 00 00 11`, `prelen +1`.
- `05` (select Scene2): `pre[0x0F] 02 -> 01` (`pre[0x0F:0x12] 01 11 00`).
- Both remain `matrix_records`; decoded record payload unchanged.

3. Track focus is branch-sensitive; `P1` focus flips `none -> tag_records`
   in these families, while `P2` focus stays `none`.
- `07` (`T2/P1`) and `09` (`T7/P1`): `none -> tag_records`, descriptor
  `1e 01 00 00`, pre-track compaction (`-6` / `-4`), Track16-family growth.
- `08` (`T2/P2`) and `10` (`T7/P2`): scene format unchanged (`none`), but
  still `+64` normalization with Track16-family growth.

Implication (high confidence for tested families):
- Active UI selection (song/scene/track/pattern focus) is serialized state,
  not just transient editor cursor state.
- Selection-only save can alter branch shape and byte layout, so scene tooling
  must compare/patch by decoded semantics within branch context, not by naive
  fixed-offset byte assumptions.

### 23) Selection Audit Pack B (`01`-`15`): Null-Save Fingerprints + No-Op Equivalence
From `output/selection-audit-pack-2026-03-03b` with user-resaved outputs in
`src` (same filenames):

1. Null-save fingerprints are family-stable:
- `unnamed150` null-save (`01`) yields normalized rewrite (`+64`, Track `9..16`
  body `+8` each).
- `91` null-save (`06`) and `bleez35` null-save (`10`) each yield
  `+4`-byte normalized rewrites with large positional churn.

2. “Already selected” actions collapse to null-save outputs:
- `04` (`T2/P1 focus`) is byte-identical to `01` (null-save) on `unnamed150`.
- `09` (`Scene3 select`) is byte-identical to `06` (null-save) on `91`.
- `13` (`Scene3 select`) is byte-identical to `10` (null-save) on `bleez35`.

3. Scene selection signaling is consistent across matrix families:
- Scene1 select (`07`, `11`): `pre[0x0F:0x12] -> 00 00 11`, with `prelen +1`.
- Scene2 select (`08`, `12`): single-byte `pre@0x0F: 0x02 -> 0x01`.

4. `bleez35` Track7 focus writes a behavior-carrying 2-byte lane change:
- `14` (`T7/P1`) and `15` (`T7/P2`) each change only `pre@0x6E/@0x6F`.
- Decoded rec3 mapping:
  - baseline: `T7=P9, T8=P2`
  - `14`: `T7=P1, T8=P9`
  - `15`: `T7=P2, T8=P9`
- This confirms coupled-lane semantics in this branch family (targeted `T7`
  focus can co-move `T8` lane state).

5. Outlier needing follow-up:
- `05_a_unnamed150_t2p2_focus` is a unique branch-shift result in this set
  (`prelen -1`, `pre[0x0F:0x12]=01 10 00`, high churn) and does not match prior
  `P2` focus behavior from the earlier family (`08_track2_pattern2_focus_from_02`).
  Treat as high-value retest target with strict pre-save UI-state logging.

## Normalized Branch Fingerprint
Several loop/mute operations enter a shared structural branch where Tracks
`9..16` are rewritten with `+8` bytes per track. This branch change can mask
small loop-only diffs unless compared within the same branch.

## Unknowns (Still Open)

1. Full pre-track record schema is partially decoded: track tag and pattern
   encoding confirmed, but internal structure of 8B compact vs 10B dual-override
   record forms not fully mapped.
2. Universal deterministic rewrite rules for normalized-branch transitions are
   not fully modeled.
3. Pattern encoding for P1 (0-based index 0) and P4+ not yet observed.
4. Track tags beyond T3/T4: tag `0x16`=T8 observed in bleez corpus but with
   different record encoding. Tags `0x1C`=T2, `0x1D`=T1, `0x19`=T5 etc.
   predicted by formula but not yet load-confirmed in scene-corpus encoding.
5. ~~Scene deletion mechanics~~ **RESOLVED** (v2-5a: inverse of creation works).
6. ~~Orphaned T16 entries~~ **RESOLVED** (v2-5b: tolerated, pre-track is authoritative).
7. Song-slot control offsets beyond tested songs (1-3) remain unverified.
8. Hybrid/matrix `R11` lane beyond `rec2[3]`: `aa/cc` coupling and
   non-`T3` targeting rules are still unresolved in `bleez35`-family files.
9. Bleez record encoding: `08 08` sub-record delimiters, `0x1F` inter-record
   separators, tag `0x16`. Structure not yet decoded — likely multi-track
   overrides with richer payload.
10. `pre[0x0F]` ordinal semantic: tracks unique override configurations
   (confirmed by file 14), but exact counting rule for bleez corpus unclear.

## Authoring Guidance (Current)

**Scene-corpus family (files 00-14):** Full programmatic scene authoring is now
viable using `xy/scene_records.py` + `xy/scene_patcher.py`:
- `patch_add_scene()` / `patch_remove_scene()` coordinate all four regions
- T1 preamble computed from record count: `0xD6 - (count + 1) * 0x21`
- Scene records require pattern-existence on target track (or `num_patterns` crash)
- Corpus-validated record forms: T3→P2 (8B), T4→P2 compact (8B), T4→P3 (9B/8B),
  T3+T4→P2 dual (10B)
- Device-tested: creation (up to 5 scenes), deletion, record swaps, song sequencing

**Full assignment API (decoded families):**
- `read_scene_assignments(project)` returns full per-scene per-track pattern maps
  (`{scene_1based: {track_1based: pattern_1based}}`).
- `patch_set_scene_assignments(project, assignments)` rewrites scene maps for:
  - scene-corpus tag-record family (`00`-`14` style sparse overrides)
  - matrix-record family (`unnamed 156` style full `T1..T8` vectors)
- For matrix files, writer preserves observed width hints (`01` vs `01 00`) where possible.
- Scene count changes are intentionally out-of-scope in this API; it rewrites the
  existing scene set only.

**Bleez-corpus family:** NOT yet supported by tooling. Different record encoding
(`08 08` delimiters, `0x1F` separators, tag `0x16`). Requires separate decoder.

**Legacy constrained modes** (still available for scaffold-based builds):
- `tools/patch_scene_song_tokens.py` and JSON build specs via
  `scene_song.{pretrack_mode,track16_mode}`.

## Related
- Narrative analysis log: `docs/logs/2026-02-14_scene_song_delta_probe.md`
- `bleez1`/`bleez2` pair analysis: `docs/logs/2026-02-28_bleez_scene_edit_pair_analysis.md`
- `bleez7`-`bleez12` scene probe analysis: `docs/logs/2026-02-28_bleez7_12_scene_probe_analysis.md`
- Scene contribution probes (`31`-`36`): `docs/logs/2026-02-28_scene_contribution_probes_31_36.md`
- Consolidated readout: `docs/logs/2026-02-28_scene_corpus_consolidated_readout.md`
- Device chain `00`-`07` analysis: `docs/logs/2026-03-01_scene_batch_a_device_chain_analysis.md`
- Cross-branch invariants: `docs/logs/2026-03-01_scene_cross_branch_invariants.md`
- Batch A follow-up `08`-`10`: `docs/logs/2026-03-01_scene_batch_a_followup_08_10.md`
- Batch A follow-up `11`-`13`: `docs/logs/2026-03-01_scene_batch_a_followup_11_13.md`
- Hypothesis pack `41`-`50`: `docs/logs/2026-03-01_scene_hypothesis_pack_41_50.md`
- Hypothesis pack `51`-`58`: `docs/logs/2026-03-01_scene_hypothesis_pack_51_58.md`
- Scene probes `72`-`77`: `docs/logs/2026-03-03_scene_probe_72_77_p5_p9_t7_candidates.md`
- Scene probes `78`-`87`: `docs/logs/2026-03-03_scene_probe_78_87_nooverride_t7_sweep.md`
- Manual resave selection-state probe: `docs/logs/2026-03-03_manual_resave_selection_state_probe.md`
- Selection audit pack B results: `docs/logs/2026-03-03_selection_audit_pack_b_results.md`
- Scene probe v1 results: `docs/logs/2026-03-01_scene_probe_v1_results.md`
- Scene probe v2 results: `docs/logs/2026-03-01_scene_probe_v2_results.md`
- Test-plan tracking: `docs/engineering/known_good_test_plan.md`
