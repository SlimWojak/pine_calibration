# Olya Calibration Session — 2026-05-19

## Session arc (start → end)

Started: orient from yesterday's stocktake (~en1gma/docs/reviews/MAP_V1_GATE_2_OLYA_PERDAY_SESSION_STOCKTAKE_2026_05_18.md). Planned 4 phases (MSS fire test → regression → narrow back → lock). Olya called pivot to verify SWH/SWL primitive first ("everything downstream of the levels defined by those"). That pivot — and what it surfaced — defined the session.

Ended: SWH/SWL primitive LOCKED. MSS canon-mirror fix #6 landed and verified. Late-fire diagnosis + parking. 5 new Phase 1 displacement-tuning anchors identified. Displacement-gate calibration **not yet started** — next session's entry point.

## What landed (committed to next Claude)

### 1. SWH/SWL primitive — LOCKED

- `port/daily_swing_params.yaml` — LOCKED by Olya 2026-05-19
- N=2, h_display=15p
- All 5 SURFACE_TEST anchors PASS (Dec 9, Dec 16, Jan 12 LINCHPIN, Jan 19, Jan 27)
- 7 regression anchors HELD (Feb 2/6/10/19/23/26 2026 + Mar 3)
- N=1 experimentally tested for 3 fringe dates (Aug 5 2025, Jan 22 2026, Feb 4 2026): catches them but introduces too much noise. Olya ruling: stay at N=2, accept the fringe-date tradeoff.
- Also Mar 31 2025 surfaced late as another N=2-uncaught structural high — same tradeoff.

### 2. Pine MSS canon-mirror fix #6 — broken-flag pattern (replaces array-remove cascade)

- `pine/htf_map_v1.pine` — fix #6 baked in
- Root cause: Pine's `array.remove(unc_sh_price, last_sh_idx)` after each break dropped the broken SH from the stack, exposing the next-older (lower) SH for the next bar to also "break" → cascading MSS fires across Mar 4-12, Apr 2-4, Apr 10-11, Apr 21-22 2025 etc.
- Canonical en1gma `mss.py:267-355` uses `broken_swings.add(swing_id)` set — keeps the SH as recent_sh but flagged as already broken. Next bar sees flag, skips.
- Fix: parallel `unc_sh_broken` array of bools in Pine. When `close > recent_sh AND not already_broken`: set broken=true, fire MSS if disp qualifies. Mirror applied to SH and SL chains.
- **Verified visually by Olya**: Mar 4-12 cascade gone, Apr 21-22 cascade gone, "bullish-MSS-on-bearish-close" examples resolved as side-effect.
- **Bug B finding**: most direction-mismatch examples (Mar 12, Apr 4, Apr 22) were consequences of cascade — fix #6 killed them too. Standalone direction-mismatch may still exist but wasn't isolable today.

### 3. Late-fire (1-day delay) — PARKED

- Pattern: Apr 2 / Jun 11 / Jun 24 / Jul 21 2025 — Olya considers day N the MSS bar, Pine fires day N+1.
- Diagnosed: upstream **swing detection granularity**. Some structural highs Olya identifies aren't N=2 fractal pivots (Mar 31 2025 was the worked example). If the SH that "should" be broken on day N isn't detected as a swing at all, Pine can't fire there.
- Resolution would require N=1 swing detection — already ruled out for noise.
- Olya verbatim: "we park the late fire issue, she can see why it happens, and resolving will introduce noise. Same issue we can see on 24th June, MSS fires a day late on 25th. Same again on 21st July MSS, fires on 22nd. These are illustrative, but implies there is a tuning required to catch the MSS on the day it happens."
- **Implication for trading**: tradeable MSS signals arrive 1 trading day late on these cases. Document this latency in any signal-quality framing.

### 4. NEW Phase 1 tuning anchors — 5 2024 dates missing displacement entirely

- May 3, May 14, Jul 3, Jul 11, Jul 23 2024
- Olya confirmed visually: NO yellow displacement dot on any of these dates, but close clearly above prior SWH wicks
- Diagnosis: displacement gates (even Daily-loose preset: atr≥1.25, body≥0.55, cg≤0.35) are too strict to detect these bars as displacement
- These are the next session's Phase 1 calibration entry — added to `port/daily_displacement_params.yaml` `verification.displacement_tuning_anchors_2024` section
- Olya verbatim: "These are look like we are detecting highs, and the close is above the SWH wicks, so it might need tuning to catch these."

## What's still open (next session entry point)

**Phase 1 displacement-gate tuning — the core unblocked goal of the original session plan, now with sharpened evidence.**

Next-session-Claude inherits:
1. The 7 original MSS_FIRE_TEST anchors from the stocktake — Jun 11/12/Oct 30/Dec 10/Dec 22 2025 + Jan 20/Mar 13 2026
   - **CAVEAT**: earlier Jun 11 PASS verdict was under pre-fix algorithm. Re-verify all 7 with fixed Pine.
   - Jun 12 was RECLASSIFIED OUT (displacement-only, not MSS). Set = 6 anchors.
2. The 5 NEW displacement_tuning_anchors_2024 (May 3/14, Jul 3/11/23 2024)
3. SURFACE_TEST (7 anchors) and REGRESSION (8 anchors) sets — must hold under whatever displacement gates Phase 1 lands on.

Tuning approach for next session:
- Start with current Daily-loose preset (atr=1.25, body=0.55, cg=0.35, override_body=0.65, override_close=0.15, allow_weak=true)
- Walk Olya through each of the 11 fire-test anchors (6 MSS + 5 displacement) — confirm fire under preset
- If preset isn't catching all: dial sliders down further. The live displacement inputs (in_7..in_13) are wired for this.
- Then regression check: 8 regression anchors must hold. If any regress, narrow back.
- Lock `port/daily_displacement_params.yaml` when all verdicts PASS.
- Port to en1gma is V005 sprint, separately G-ratified per stocktake §5 step_3.

## Reference state at checkpoint

**Chart state (live TV instance):**
- FX:EURUSD 1D
- Indicator entity_id: `mVF5sg`
- Inputs at checkpoint:
  - `in_0` (i_swing_n) = **2** (LOCKED)
  - `in_6` (i_loose_preset) = **true** (Daily-loose ON)
  - `in_14` (i_show_swings) = true
  - `in_16` (i_show_mss) = true
  - Others at defaults
- Visible range: roughly May 2024 (the missing-fire investigation final state)
- State table reads: `N=2, h>=15p disp / atr>=1.25, body>=0.55, cg<=0.35 [LOOSE]`

**Pine source state:**
- `pine/htf_map_v1.pine` — fix #6 applied. ARMED for next session.
- TV cloud-saved version: also fix #6 (pushed + compiled this session).
- Two minor in-session simplifications during the source push (shortened legend, trimmed input tooltips). Behavior identical — visual only.

**Input-ID gotcha for next-session-Claude:**
`indicator_set_inputs` rejects Pine variable names. Use zero-indexed `in_N`:
- in_0 i_swing_n
- in_1 i_min_height_pips
- in_2 i_swing_date_labels
- in_3 i_flip_mode
- in_4 i_atr_length
- in_5 i_use_suppression
- in_6 i_loose_preset
- in_7 i_atr_mult
- in_8 i_body_ratio
- in_9 i_close_gate
- in_10 i_override_body
- in_11 i_override_close
- in_12 i_override_pip
- in_13 i_override_allow_weak
- in_14 i_show_swings
- in_15 i_show_displacement
- in_16 i_show_mss
- in_17 i_show_direction
- in_18 i_show_df_events
- in_19 i_show_pivotal
- in_20 i_show_table
- in_21 i_show_weak_disp
- in_22 i_show_legend

**Source-push gotcha:**
- `pine_set_source` requires Pine editor open AND a script loaded. Sequence: `ui_open_panel pine-editor` → `pine_open` (loads saved script) → `pine_set_source` → `pine_smart_compile`.
- After `pine_smart_compile` saves, the chart-loaded indicator auto-updates (no manual "Add to chart" needed, contrary to the older CLAUDE.md note from iter 4).

## Git state

To be committed at end of this checkpoint:
- pine/htf_map_v1.pine (fix #6)
- port/daily_displacement_params.yaml (session findings + new 2024 tuning anchors)
- port/daily_swing_params.yaml (LOCKED, was created earlier in session)
- scorecard.md (swing row marked LOCKED earlier; may need displacement row touch-up)
- notes/olya_session_2026_05_19.md (this file)
- Several investigation screenshots in ~/mcp-servers/tradingview-mcp/screenshots/ (NOT in repo — referenced by path)

## What this session does NOT do

Per stocktake §5 step_3 and en1gma CLAUDE.md §6:
- Does NOT write to `~en1gma/console/detection/locked_baseline.yaml` (V005 sprint scope)
- Does NOT promote any FVG/OB/BPR work (§6 standing stop)
- Does NOT touch M002 NEX Weekly Terrain (separate G-routing decision)
- Pine session output: 2 LOCKED YAMLs (swing + displacement) handed to V005 sprint when both locked.

---

## EVENING ADDENDUM (2026-05-19) — Canon-mirror audit fixes #7-11 + noise sweep

Triggered by Olya's question: **"why no MSS on May 3/14, Jul 3/11/23 2024?"** Previous-Claude
hypothesized displacement tuning. This session re-anchored on Olya's "Pine must mirror canon
LOGIC; params may be tuned to HTF" principle, audited Pine vs canon, ran a noise sweep, and
identified 5 additional canon-mirror drifts (fixes #7-11) that the prior audit missed.

### Fixes landed (commit at end of evening)

| # | Primitive | Drift | Fix |
|---|-----------|-------|-----|
| #7 | displacement | `allow_weak_override` Pine-only knob (canon `displacement.py:337` gates override on grade=None ONLY) | Removed the knob; override now strictly canon-locked to `single_grade == ""` (atr_ratio < 1.25) |
| #8 | displacement | cluster-2 `net_efficiency` denominator used `combined_range` (max-min); canon `displacement.py:158` uses `(r0 + r1)` (SUM of individual ranges) | Pine now divides by `(prior_range + candle_range)` → gate is canon-tight, no longer artificially loose |
| #9 | displacement | OR-flat path eval with single suppressing cluster (`not qualifies_and AND not prior_qualifies_and`) | Restructured to canon order: cluster → single → override. Cluster evaluated without suppression; single suppressed when cluster fires; override only when not cluster AND grade=None |
| #10 | mss | Pine only searched displacement on `[i-1, i]`; canon HTF `mss.py:42-63` window=1 searches `[i-1, i, i+1]` (with +1 lookahead) | Added pending-break state: on close>SH with no immediate disp, mark pending; on next bar if disp fires bull, retroactively emit MSS at the pending bar. 1-bar lookahead = canon HTF `confirmation_window_bars=1` |
| #11 | mss | Pine marked SH as broken on ANY `close > recent_sh` regardless of MSS firing; canon `mss.py:267` only adds to `broken_swings` when `confirmed_disp` is found | Pine now sets `unc_sh_broken` ONLY when MSS actually fires. Required for fix #10 to work (otherwise pending lookahead would find an already-broken SH) |

### Noise-sweep findings (1Y EUR/USD 2024, 251 bars, 6E=F daily as proxy)

Pure parameter tuning to "catch the 5 anchors" requires unacceptable noise inflation:

| Config | Description | Disp fires/yr | Noise% | Anchors caught |
|--------|-------------|---------------|--------|----------------|
| **B1** | Canon HTF locked (atr=1.5/body=0.65/cg=0.25) | **30** | **12.0%** | 0/5 |
| B2 | Daily-loose preset (current — atr=1.25/body=0.55/cg=0.35) | 68 | 27.1% | 0/5 |
| S4 | catch-all (atr=1.05/body=0.42/cg=0.55) | 90 | 35.9% | 4/5 |
| S3 EXP | proposed HTF expansion-bar canon amendment (atr≥1.25 + close-side≥0.5) | 88 | 35.1% | 2/5 |

**Key insight:** Daily-loose preset currently in Pine adds 2.3× canon's noise WITHOUT catching ANY anchors.
Considered a wasted cost — under canon-mirror logic the slider tuning isn't helping.

### Anchor reclassification post fix #10

Neighborhood analysis (canon-HTF gates, ±5 bars) shows:

| Anchor | Pre-fix-#10 (Pine [i-1, i]) | Post-fix-#10 (canon [i-1, i, i+1]) | Resolution |
|--------|------------------------------|-------------------------------------|------------|
| 2024-05-03 | miss | miss | **Park** — no canon-grade disp in ±5 bars |
| 2024-05-14 | HIT (May 13 OVR-bull) | HIT | Should fire post-fix (TV-side verification needed; data divergence between 6E=F and FOREXCOM possible) |
| 2024-07-03 | miss | **HIT** (gained — disp at i+1) | Fix #10 catches |
| 2024-07-11 | miss | **HIT** (gained — disp at i+1) | Fix #10 catches |
| 2024-07-23 | miss | miss | **Park** — no canon-grade disp in ±5 bars |

**Score:** Pine pre-fix = 1/5. Fix #10 canon-mirror = **3/5**. Remaining 2 (5/3, 7/23) require V003 park
or V005 canon amendment — not Pine drift; truly outside canonical displacement methodology.

### MSS dependency map (for orientation)

```
MSS depends on:
├── swing_points   → LOCKED (N=2) — defines what gets broken
├── displacement   → REQUIRED — gate (within confirmation window)
└── fvg            → tag only (NOT a gate)

Tunable per locked_baseline.yaml#mss:
- displacement_required: true       ← locked methodology (can't disable)
- close_beyond_swing: true          ← locked methodology
- confirmation_window_bars: 1 (HTF) ← TUNABLE per TF (LTF=3)
- structure_close_required: true    ← locked methodology
- swing_consumption: true           ← locked methodology
- fvg_tag_only: true                ← tag-only by design
```

Only `confirmation_window_bars` is a clean per-TF tuning lever. Pine pre-fix had it
effectively at 0 (lookback only, no lookahead). Post-fix-#10 matches canon's HTF=1.
**Widening further (to 2/3/5) does NOT help on the 2024 anchors** — neighborhood test shows.

### Realtime tradeoff documentation (Fix #10)

Per canon HTF MSS semantics: when displacement confirms on bar+1, MSS is LABELED at the
break bar but DETECTED at bar+1. In Pine real-time:
- Break bar i: pending state set, no label yet
- Bar i+1: disp fires → MSS label appears at bar i (retroactively)

This is the "MSS fires 1 day late" observation from earlier — now resolved as **canon-faithful
behavior, not a Pine bug**. Trading signal latency: 1 trading day on these cases. Acceptable
under V003 ("major moves > micro moves").

### Updated session deliverables

- `pine/htf_map_v1.pine` — fixes #7-11 applied, compiled clean on TV (pine_smart_compile success)
- `port/daily_displacement_params.yaml` — anchor verdicts updated (TBD verdicts → POST-FIX status)
- This file — full evening addendum
- `/tmp/disp_diagnostic.py`, `/tmp/disp_sweep.py`, `/tmp/mss_window_test.py`, `/tmp/may14_debug.py` — investigation harnesses (not in repo)

### Open items for Olya next session

1. **Visual verify Jul 3, Jul 11 2024 NOW show MSS labels** (gained from fix #10)
2. **Visual verify May 14 2024 MSS** (should appear; if not, FOREXCOM:EURUSD vs 6E=F divergence on May 13)
3. **Methodology ruling on May 3, Jul 23 2024:**
   - Option C: Park as "not canonical displacement under HTF" (V003)
   - Option D: V005+ amendment for a new "extension_bar" primitive that captures wide-range mid-close days
4. **Decision on Daily-loose preset:** revert to canon HTF (atr=1.5/body=0.65/cg=0.25) since current loose adds 2.3× noise without benefit. Lock displacement YAML at canon values.

### What this session does NOT do (still in force per CLAUDE.md §6)

- No write to `~en1gma/console/detection/locked_baseline.yaml` — V005 sprint scope
- No FVG/OB/BPR/IFVG/composite PDA work
- No M002 NEX Weekly Terrain incorporation
- No methodology change — all fixes are canon-mirror (catching Pine drift, not changing canon)
