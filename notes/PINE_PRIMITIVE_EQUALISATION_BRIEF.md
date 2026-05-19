# PINE PRIMITIVE EQUALISATION BRIEF

document: PINE_PRIMITIVE_EQUALISATION_BRIEF
version: 1.1
date: 2026-05-19
status: DRAFT - CODEX GOAL TUNED - awaiting G ratification before dispatch
owner: CTO (en1gma)
classification: PINE_CALIBRATION_SANDBOX | PRIVATE_INERT | NO_EN1GMA_WRITES
target_repo: pine_calibration (NOT en1gma)
runner: Codex GOAL
phase_scope: PHASE_A_ONLY
ratification_anchor: TBD on G ratification
purpose: Prove that pine_calibration/pine/htf_map_v1.pine is a strict canon mirror for the three already-ported Phase A primitives: swing_points, displacement, and mss. The proof is signal-diff parity against research_accelerator cascade output over the fixed Daily window, with per-primitive audit receipts and transcript-visible evidence for GOAL evaluation. Phase B primitives are explicitly deferred to a separate dispatch after this harness is proven.

---

## 0. Fresh-context orientation

Olya is a chart trader and TradingView spatial-pattern expert. en1gma visualises HTF Map V1 output on her TV terminal through pine_calibration/pine/htf_map_v1.pine. Olya time must be spent judging methodology, not debugging implementation drift. Therefore Pine must mirror the canonical Python detection logic exactly for the primitives Map V1 consumes.

This dispatch is Phase A only: prove the existing Pine implementations for swing_points, displacement, and mss by signal-diff against research_accelerator over EUR/USD Daily 2024-01-01..2026-03-31. Do not extend to FVG, reference_levels, or htf_liquidity in this GOAL run. Do not write to en1gma. Do not change methodology. Do not tune parameters to visual anchors. CODE WINS.

---

## 1. Codex GOAL target

### 1.1 Primary goal

Pine source pine_calibration/pine/htf_map_v1.pine mirrors canonical en1gma detection logic for Phase A primitives only:

- swing_points
- displacement
- mss

Mirror status must be proven by signal-diff against research_accelerator cascade output across the fixed Daily window 2024-01-01..2026-03-31 with zero unexplained divergences, and anchored by audit receipts committed in pine_calibration.

### 1.2 Explicit non-goal

Phase B is not part of this GOAL run. Do not implement or audit these primitives in this dispatch:

- fvg
- reference_levels
- htf_liquidity

Phase B receives a separate brief after Phase A proves the harness, data-source contract, Pine extraction method, and receipt pattern.

### 1.3 Exit criteria: ALL must hold

EXIT_0 - Gate 0 preflight receipt exists at pine_calibration/notes/GOAL_GATE0_PREFLIGHT.md and is committed. It records: anchor hashes, fixed Daily window, River smoke-test command/result, TV cloud-save verification, Codex GOAL runner, displacement LOCKED decision, Phase A-only scope, and V005 hold confirmation.

EXIT_1 - Audit receipts exist and are committed:

- pine_calibration/notes/canon_audit_swing_points.md
- pine_calibration/notes/canon_audit_displacement.md
- pine_calibration/notes/canon_audit_mss.md

Each receipt contains: signal-diff CSV path, divergence counts by class, line-by-line canon citation table, streaming-vs-precompute notes where applicable, anchor commit tuple, date, and operator.

EXIT_2 - Signal-diff CSVs exist at pine_calibration/diffs/ for all three Phase A primitives and show zero unexplained divergences over 2024-01-01..2026-03-31. Explained divergences are allowed only if documented in the corresponding receipt.

EXIT_3 - Pine compiles cleanly on TradingView via pine_smart_compile. The compile output is captured in the transcript and final summary.

EXIT_4 - port/daily_swing_params.yaml remains LOCKED. port/daily_displacement_params.yaml is LOCKED at canon HTF values per CODE WINS plus 2026-05-19 noise-sweep evidence. port/daily_mss_params.yaml exists and is LOCKED at canon HTF values.

EXIT_5 - scorecard.md is updated with one row per Phase A primitive showing PASS plus anchor commit hash.

EXIT_6 - notes/GOAL_RUN_LEDGER.md is updated by the orchestrator with final state, accepted subagent findings, rejected findings if any, validators, commits, and next Phase B recommendation.

EXIT_7 - Final summary commit exists with comparison table across the three Phase A primitives, signal-diff totals, compile receipt, anchor tuple, and recommended next-Olya-session entry point.

### 1.4 Hard cap

If exit criteria are not met after 30 turns or 8 hours wall-clock, stop and report state. Do not invent completion.

---

## 2. Gate 0: fixed preflight contract

Gate 0 must complete before primitive work starts. If any Gate 0 item cannot be verified, halt before editing Pine.

GATE0_1 - Daily window is fixed:

- start: 2024-01-01
- end: 2026-03-31
- symbol intent: EUR/USD Daily

GATE0_2 - River coverage is verified by smoke test from research_accelerator. Required command pattern:

```bash
cd ~/research_accelerator
python3 site/detect.py --start 2024-01-01 --end 2026-03-31
```

The exact command actually used and non-empty output evidence must be written to GOAL_GATE0_PREFLIGHT.md. If the local CLI requires additional config or symbol flags, record the exact resolved command.

GATE0_3 - TradingView cloud-save state is verified before edits. Open the Pine editor and confirm the cloud version SHA matches local pine_calibration/pine/htf_map_v1.pine. Record the verification method and matched SHA in GOAL_GATE0_PREFLIGHT.md.

GATE0_4 - Runner choice is fixed to Codex GOAL. No Claude GOAL, MISSION fallback, or alternate sprint runner in this dispatch unless G re-ratifies.

GATE0_5 - Displacement YAML lock state is resolved: LOCKED at canon HTF values, justified by CODE WINS plus the 2026-05-19 noise-sweep evidence. Do not route displacement back to Olya for implementation parity.

GATE0_6 - Phase split is resolved: Phase A first, Phase B separate dispatch.

GATE0_7 - en1gma hash freeze is confirmed: V005 sprint is held until this brief completes. Record en1gma HEAD at start and verify unchanged at end.

GATE0_8 - research_accelerator HEAD and pine_calibration HEAD are recorded at start. If either moves unexpectedly during the run, halt and report stale anchor risk.

---

## 3. Authority and orientation surface

Read these first, in order:

- ~/en1gma/CLAUDE.md - kernel orientation; section 6 standing stops bind this sandbox by reference
- ~/en1gma/docs/canonical/DETECTION_PRIMITIVES_INDEX.md - CODE WINS rule, primitives index, vLOCK status
- ~/en1gma/docs/reviews/PHASE_5_C2_DAILY_LED_HTF_MAP_V1_METHODOLOGY_BASELINE_RATIFIED_2026_05_05.md - Map V1 component-to-primitive mapping
- ~/pine_calibration/CLAUDE.md - sandbox rules, canon-mirror discipline, Pine workflow notes
- ~/pine_calibration/notes/olya_session_2026_05_19.md - fixes #7-#11, displacement/MSS session evidence
- ~/pine_calibration/notes/PINE_PRIMITIVE_EQUALISATION_BRIEF.md - this brief

Canon source of truth:

- ~/en1gma/en1gma/console/detection/locked_baseline.yaml
- ~/en1gma/en1gma/console/detection/ra_engine/detectors/swing_points.py
- ~/en1gma/en1gma/console/detection/ra_engine/detectors/displacement.py
- ~/en1gma/en1gma/console/detection/ra_engine/detectors/mss.py
- ~/en1gma/en1gma/console/detection/ra_engine/config/schema.py

Canon execution oracle:

- ~/research_accelerator/README.md
- ~/research_accelerator/run.py
- ~/research_accelerator/configs/locked_baseline.yaml
- ~/research_accelerator/site/detect.py
- ~/research_accelerator/src/ra/data/river_adapter.py

Pine sandbox:

- ~/pine_calibration/pine/htf_map_v1.pine
- ~/pine_calibration/port/daily_swing_params.yaml
- ~/pine_calibration/port/daily_displacement_params.yaml
- ~/pine_calibration/scorecard.md

Reference UX target:

- ~/en1gma/docs/draft_map_html/MAP_V1_GATE_2_DAILY_STATE_TABLE_PERDAY_2026_05_18.html

---

## 4. Thin orchestration protocol

The main agent is the CTO/orchestrator. It owns context, authority, sequencing, integration, verification, and user-facing judgment.

The orchestrator must maintain pine_calibration/notes/GOAL_RUN_LEDGER.md. It is the durable operating picture and must contain:

- current gate and primitive
- anchor hashes
- delegated tasks
- accepted findings
- rejected or superseded findings
- files changed
- signal-diff status
- compile status
- validator status
- next action
- halt risk, if any

Subagents may be used only for bounded specialist work. They may:

- inspect one primitive
- build a citation table
- inspect one detector/config pair
- implement within assigned pine_calibration file ownership
- run a validator and report evidence

Subagents may not:

- ratify PASS
- change scope
- interpret methodology
- write under ~/en1gma or ~/research_accelerator
- create new authority surfaces
- mark Olya-facing status
- decide HALT vs PASS

The orchestrator must integrate subagent work before commit. A subagent report is evidence, not authority.

---

## 5. Scope and write fences

### 5.1 In scope for this GOAL

- Verify and, only if needed, patch Pine swing_points to mirror canon.
- Verify and, only if needed, patch Pine displacement to mirror canon.
- Verify and, only if needed, patch Pine mss to mirror canon.
- Build/reuse signal-diff extraction and CSVs needed for those three primitives.
- Produce audit receipts, YAML lock files, scorecard rows, run ledger, and final summary.

### 5.2 Deferred to Phase B

- fvg
- reference_levels
- htf_liquidity

Phase B is expected next, but this GOAL must not start it.

### 5.3 Explicitly out of scope

- order_block
- ote
- liquidity_sweep
- session_liquidity
- asia_range
- equal_hl
- ifvg as a separate detector
- bpr as a separate detector
- luxalgo_ob
- luxalgo_mss
- any methodology change
- any en1gma canon write
- any V005 sprint preempt

### 5.4 Allowed writes

- ~/pine_calibration/pine/htf_map_v1.pine
- ~/pine_calibration/port/daily_swing_params.yaml
- ~/pine_calibration/port/daily_displacement_params.yaml
- ~/pine_calibration/port/daily_mss_params.yaml
- ~/pine_calibration/notes/GOAL_GATE0_PREFLIGHT.md
- ~/pine_calibration/notes/GOAL_RUN_LEDGER.md
- ~/pine_calibration/notes/canon_audit_swing_points.md
- ~/pine_calibration/notes/canon_audit_displacement.md
- ~/pine_calibration/notes/canon_audit_mss.md
- ~/pine_calibration/notes/PINE_PRIMITIVE_EQUALISATION_BRIEF.md, status/hash fields only after ratification
- ~/pine_calibration/diffs/*.csv
- ~/pine_calibration/scorecard.md

### 5.5 Forbidden writes

- ~/en1gma/**
- ~/research_accelerator/**
- TradingView cloud script under a new name
- Any new canonical doc
- Any methodology/ruling/promotion surface

---

## 6. Per-primitive artifact set

For each primitive in {swing_points, displacement, mss}, produce:

ART_1 - Pine source mirror in pine_calibration/pine/htf_map_v1.pine. Existing code should be preserved unless signal-diff proves drift.

ART_2 - Signal-diff CSV at pine_calibration/diffs/<primitive>_canon_vs_pine.csv with columns:

```text
date,primitive_event_type,canon_present,pine_present,canon_properties,pine_properties,divergence_class
```

Allowed divergence_class values:

```text
NONE
CANON_ONLY
PINE_ONLY
PROPERTY_MISMATCH
EXPLAINED_DATA_SOURCE
EXPLAINED_WARMUP
EXPLAINED_GHOST_BAR
EXPLAINED_COSMETIC
```

Convergence target: zero rows in {CANON_ONLY, PINE_ONLY, PROPERTY_MISMATCH}.

ART_3 - Audit receipt at pine_calibration/notes/canon_audit_<primitive>.md with:

- canon source citations with file paths and line numbers
- locked_baseline.yaml parameter reference
- Pine source line range
- Pine construct-to-canon citation table
- streaming-vs-precompute notes
- signal-diff summary
- anchor tuple: pine_calibration HEAD, en1gma HEAD, research_accelerator HEAD
- date and operator

ART_4 - Port YAML status:

- swing_points: LOCKED, N=2, height_filter_pips=15.0
- displacement: LOCKED, atr_multiplier=1.5, body_ratio=0.65, close_gate=0.25, structure_close_required=true, decisive_override body_min=0.75, close_max=0.10, pip_floor_1D=20.0, allow_weak_grade=false
- mss: LOCKED, displacement_required=true, close_beyond_swing=true, confirmation_window_bars=1, structure_close_required=true, swing_consumption=true, fvg_tag_only=true

ART_5 - scorecard.md row:

```text
primitive | canon_mirror_status | signal_diff_anchor_commit | port_yaml_status | next_step
```

---

## 7. Mechanical loop

Run primitives in this order:

1. swing_points
2. displacement
3. mss

For each primitive:

STEP_1 - Read canon source and locked_baseline.yaml section.
STEP_2 - Read Pine block.
STEP_3 - Build citation table: Pine line -> Pine logic -> canon line -> MIRROR / MAP_VISUAL / COSMETIC / STREAMING_ACCOMMODATION.
STEP_4 - Run canon cascade over 2024-01-01..2026-03-31 and extract primitive events.
STEP_5 - Extract Pine events from TradingView using a machine-readable table or data_get_pine_tables equivalent.
STEP_6 - Diff by date/event/property and emit CSV.
STEP_7 - If unexplained divergences exist, inspect OHLC on River and FOREXCOM first. Classify data-source divergence where justified; otherwise patch Pine, compile, re-extract, re-diff.
STEP_8 - When zero unexplained divergences remain, write receipt.
STEP_9 - Update port YAML, scorecard, and GOAL_RUN_LEDGER.md.
STEP_10 - Commit one primitive anchor.
STEP_11 - Post a GOAL_EVIDENCE_BLOCK in transcript.

Do not move to the next primitive until the current primitive has a receipt, CSV, ledger update, and commit.

---

## 8. Pine evidence extraction contract

Forensic parity requires machine-readable Pine evidence. Prefer a stable Pine table or TradingView MCP data extraction over screenshots.

Required Pine-side event fields:

```text
date
primitive
event_type
direction
price_or_level
source_bar_index
state
key_properties_json
```

Screenshot OCR is allowed only as a last-resort fallback. If OCR is used, record why machine extraction failed and mark OCR as a residual risk in the receipt and final report.

---

## 9. Data-source contract

Canon input: River EUR/USD 1D parquet via research_accelerator River adapter.

Pine input: FOREXCOM:EURUSD 1D on TradingView.

These are not guaranteed byte-identical. Data-source differences are not Pine drift.

For each candidate CANON_ONLY, PINE_ONLY, or PROPERTY_MISMATCH:

- fetch River OHLC for the event date
- fetch FOREXCOM OHLC for the event date
- if H/L differ by more than 1 pip or close differs by more than 0.5 pip on a relevant field, classify EXPLAINED_DATA_SOURCE
- if OHLC agrees inside tolerance, treat as real Pine drift and route to patch loop

If EXPLAINED_DATA_SOURCE rows exceed 5% of total events for any primitive, halt and report HALT_2.

---

## 10. Streaming-vs-precompute policy

Canon Python detectors may precompute upstream state arrays before iteration. Pine streams bar by bar. Literal line-by-line ports can be wrong when canon accesses future/precomputed state that Pine cannot yet know.

MSS fix #10/#11 is the governing example: canon mss.py with confirmation_window_bars=1 searches disp_by_idx in [i-1, i, i+1]. Python can do this because disp_by_idx is precomputed. Pine must defer the MSS decision one bar via pending-break state, and must defer broken_swings.add() until MSS actually fires. The end-state mirrors canon; literal same-line behavior would not.

Policy:

POL_1 - Identify every canon precompute dependency.
POL_2 - Document the Pine streaming accommodation.
POL_3 - Judge mirror by finalised end-state, not intra-bar behavior.
POL_4 - Streaming accommodation is not drift if signal-diff proves parity.
POL_5 - Failure to document an accommodation is drift.
POL_6 - Halt only if signal-diff remains divergent after multiple Pine-native patterns have been tried.

---

## 11. Validators

VAL_1 - Gate 0 preflight receipt exists and has no TBD fields except ratification anchor before G signs.

VAL_2 - Pine compiles clean via pine_smart_compile.

VAL_3 - Signal-diff zero unexplained for swing_points, displacement, and mss.

VAL_4 - Audit receipts complete and line citations resolve.

VAL_5 - Anchor tuple valid: pine_calibration HEAD, en1gma HEAD, research_accelerator HEAD.

VAL_6 - en1gma HEAD unchanged from Gate 0 to final.

VAL_7 - research_accelerator cascade replay is deterministic for the fixed window.

VAL_8 - port/*.yaml parses as valid YAML and matches canon HTF values.

VAL_9 - No en1gma writes.

VAL_10 - No methodology language in receipts: no rulings, no "Olya should accept", no new thresholds or formulas.

VAL_11 - GOAL_RUN_LEDGER.md final state matches committed artifacts.

---

## 12. Halt conditions

HALT_1 - Methodology question surfaces. Route to G/Olya; do not answer in implementation sprint.

HALT_2 - Data-source divergence exceeds 5% of total events for any primitive.

HALT_3 - Pine v6 limitation prevents end-state parity.

HALT_4 - Two consecutive Phase A primitives fail to converge after patch attempts.

HALT_5 - Any attempt to write under ~/en1gma/.

HALT_6 - Section 6 standing-stop crossing attempt: B01, runtime fanout, OB/BPR/IFVG composite PDA, certification, paper-trading, methodology promotion, or related scope.

HALT_7 - Olya methodology baseline change attempt.

HALT_8 - V005 sprint preempt or en1gma canon parameter write.

HALT_9 - Canon cascade replay is not deterministic.

HALT_10 - Hard cap reached: 30 turns or 8 hours.

HALT_11 - TV cloud-save state cannot be verified before Pine edits.

When halting:

- stop new implementation
- write pine_calibration/notes/HALT_REPORT_<date>_<halt_id>.md
- update GOAL_RUN_LEDGER.md
- commit current pine_calibration state if useful and non-destructive
- report condition, state reached, attempted fixes, required human decision, and recommended next step

---

## 13. Non-authorizations

This brief does NOT authorize:

NA_1 - any en1gma write of any kind
NA_2 - any research_accelerator write of any kind
NA_3 - any methodology change, ruling, promotion, or retirement
NA_4 - any new vLOCK primitive
NA_5 - any V005 sprint preempt
NA_6 - shadow / paper / certification / runtime / public / B01 / accepted-ref / inventory / SAFE_LEAF_MATCH movement
NA_7 - OB / BPR / IFVG-as-separate-primitive / composite PDA work
NA_8 - Olya methodology ruling without G ratification
NA_9 - new canonical doc in en1gma
NA_10 - new authority surface for any subagent
NA_11 - parameter retuning beyond canon HTF Phase A locks
NA_12 - cosmetic Pine refactor unrelated to canon-mirror proof
NA_13 - changing Pine swing N or h_display from LOCKED 2026-05-19 values
NA_14 - changing Pine MSS confirmation_window_bars from 1
NA_15 - removing fix #11 streaming accommodation under mistaken literal-mirror logic
NA_16 - starting Phase B implementation in this GOAL run

---

## 14. Codex GOAL invocation

### 14.1 Anchor fields

anchor_pine_calibration_hash: TBD
anchor_en1gma_hash: TBD
anchor_research_accelerator_hash: TBD
anchor_daily_window_start: 2024-01-01
anchor_daily_window_end: 2026-03-31
anchor_runner: Codex GOAL
anchor_phase_scope: PHASE_A_ONLY
anchor_v005_status: HELD_PENDING_PHASE_A_COMPLETION

### 14.2 Verbatim GOAL directive

Prove Phase A canon-mirror parity for pine_calibration/pine/htf_map_v1.pine against canonical en1gma detection logic for swing_points, displacement, and mss only. Use fixed EUR/USD Daily window 2024-01-01..2026-03-31. Complete Gate 0 first: record pine_calibration, en1gma, and research_accelerator hashes; verify River coverage with research_accelerator site/detect.py smoke test; verify TradingView cloud-save state matches local Pine; confirm runner is Codex GOAL; confirm displacement YAML is LOCKED per CODE WINS plus 2026-05-19 noise-sweep evidence; confirm Phase B is deferred; confirm V005 is held and en1gma HEAD frozen. Maintain notes/GOAL_RUN_LEDGER.md as the orchestrator state ledger. For each primitive, produce signal-diff CSV with zero unexplained divergences, canon_audit_<primitive>.md receipt with line citations and streaming accommodations, locked port YAML status, scorecard row PASS, commit, and transcript GOAL_EVIDENCE_BLOCK. Pine must compile clean via pine_smart_compile. Final output is a summary commit and transcript table across the three Phase A primitives. Stop if all EXIT_0..EXIT_7 in notes/PINE_PRIMITIVE_EQUALISATION_BRIEF.md hold, or any HALT_1..HALT_11 triggers, or 30 turns/8 hours elapse. Non-authorizations: no en1gma writes, no research_accelerator writes, no methodology change, no canon promotion, no V005 preempt, no Phase B implementation, no OB/BPR/IFVG composite work, no shadow/paper/cert/runtime movement.

### 14.3 Transcript evidence block

After Gate 0 and after each primitive, post this block in the transcript:

```text
GOAL_EVIDENCE_BLOCK
gate_or_primitive:
window:
anchor_pine_calibration:
anchor_en1gma:
anchor_research_accelerator:
canon_events:
pine_events:
unexplained_divergences:
explained_data_source:
explained_warmup:
explained_ghost_bar:
explained_cosmetic:
compile_status:
receipt_path:
diff_csv_path:
commit_hash:
ledger_state:
next_action:
```

The GOAL evaluator reads transcript, not files. If the evidence is not posted, completion may not be recognised even when artifacts exist.

---

## 15. Commit and report contract

### 15.1 Expected primitive commits

```text
feat(pine): swing_points canon-mirror anchor + receipt
feat(pine): displacement canon-mirror anchor + receipt
feat(pine): mss canon-mirror anchor + receipt
```

Each commit includes the relevant Pine source delta if any, signal-diff CSV, audit receipt, port YAML, scorecard row, and ledger update.

### 15.2 Final summary commit

```text
docs(pine): phase-a canon-mirror equalisation summary
```

Commit body must include:

- comparison table: primitive | canon events | Pine events | unexplained | explained-data-source | explained-warmup | explained-ghost | explained-cosmetic | port YAML status
- anchor tuple
- Daily window
- pine_smart_compile receipt
- validator results
- explicit non-authorizations preserved
- recommended Phase B dispatch entry point
- recommended next-Olya-session entry point

### 15.3 Final transcript report

Post the comparison table inline after final summary commit. Include validator status and next lane. Keep it concise but evaluator-visible.

---

## 16. Phase B holding lane

Phase B follow-up should be a separate GOAL brief for:

- reference_levels
- htf_liquidity
- fvg

Recommended order remains reference_levels -> htf_liquidity -> fvg. Phase B should reuse the Phase A signal-diff harness, ledger pattern, and evidence block. Phase B params should be DRAFT at canon HTF values pending Olya verification unless separately ratified.

---

## 17. Anti-patterns

ANTI_1 - Do not tune Pine sliders to catch Olya anchors.
ANTI_2 - Do not mark PASS without signal-diff.
ANTI_3 - Do not ask Olya to verify canon-mirror.
ANTI_4 - Do not write to en1gma.
ANTI_5 - Do not write to research_accelerator.
ANTI_6 - Do not rebuild the indicator from scratch.
ANTI_7 - Do not revert fixes #1-#11.
ANTI_8 - Do not literalise mss.py:355 against the streaming policy.
ANTI_9 - Do not begin Phase B early.
ANTI_10 - Do not promote HALT reports to authority.

---

## 18. Version history

- 1.0 - 2026-05-19 - Initial brief drafted post-evening-canon-audit. Author: Opus advisor session in Cursor. Status: DRAFT pending G ratification.
- 1.1 - 2026-05-19 - Tuned for Codex GOAL Phase A dispatch. Added Gate 0 contract, thin orchestration protocol, GOAL run ledger, machine-readable Pine evidence contract, transcript evidence block, Phase A-only exit criteria, and Phase B holding lane. Author: G/Codex.
