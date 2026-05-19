# Olya Calibration Session — 2026-05-17 (resumed afternoon)

## Session shape

First half (morning, paused ~14:30): Daily swing param tuning. Iterated N from 2→1, h from 15→3p. Olya called "still missing Feb 10 SWH, Feb 19 SWL; spurious noise at Feb 5/6/9/10".

Afternoon resumption: stepped back, **audited the Pine sandbox against the canonical en1gma detectors** (`swing_points.py`, `displacement.py`, `mss.py`, `locked_baseline.yaml`). Found multiple deviations. Landed 4 algorithmic fixes + 1 UX bundle.

## What we landed

| Fix | What | Why |
|---|---|---|
| **swing_points** | Emit ALL N-pivots; height_pips = distance to nearest prior opposite swing | Old impl filtered pivots inline by immediate-neighbor pips, killing cluster-top SWHs (Feb 10 was only 1.4p above Feb 09/11). Canonical emits all, computes height as leg amplitude. |
| **MSS Fix #1** | Check ONLY most-recent prior swing (mss.py:254-264) | Old impl iterated all unconsumed swings, causing Feb 18 to fire bear MSS against Feb 02 SWL. Olya ruled Mar 02 (vs Feb 19 SWL) is the real bearish flip — canonical most-recent-only matches her ruling exactly. |
| **MSS Fix #2** | Impulse suppression (mss.py:215-251) | Canon-faithful structure: retrace / opp-disp / new-day reset. No-op on Daily (new_day_reset clears each bar) but in place for LTF port. |
| **Disp Fix #3** | ATR grade taxonomy (STRONG/VALID/WEAK), qualifies grid keyed at "atr1.5_br0.6" | Old impl used a fixed gate. Canonical has graded grid + decisive override path. |
| **Disp Fix #4** | Cluster-2 strict progression (`h1>h0 & l1>=l0` bull, mirrored bear) + overlap ≤ 0.35 + override pip_floor 20p (1D) | Loose cluster previously fired on any 2 same-direction bars meeting net_eff. |
| **UX bundle** | Toggleable legend (bottom-left), tooltips on every input, single yellow dot for displacement (bold=STRONG, faded=qualified), MSS lines removed, LIVE displacement gate inputs, Daily-loose preset toggle | Olya wanted clearer chart, ability to dial gates live |

## Rulings captured

- **Q1 (Mar 02 is the bearish flip)** — close goes below Feb 19 SWL @ 1.17418 on Mar 02 (close 1.16129). Feb 18 close 1.17731 is *above* Feb 19 SWL — no flip. Verified visually after MSS Fix #1.
- **Swing methodology** — canonical algo at canonical defaults (N=2, h_display=15p) correctly catches Feb 10 SWH (163p) and Feb 19 SWL (186p), plus Feb 02 SWL (306p), Feb 06 SWL (317p), Feb 23 SH (92p), Feb 26 SH (87p), Mar 03 SL (298p). Olya signed off visually.
- **MSS lines** — visual diagnostic only, not used to compute anything downstream. Olya didn't find them useful → removed.

## Open issue — missing-MSS calibration

Olya pointed to specific dates where she expects MSS but algorithm doesn't fire:

| Date | Bar | Range | ATR | atr_ratio | body | close-loc | Why displacement failed (canonical gates) |
|------|-----|-------|-----|-----------|------|-----------|---|
| Jun 11 2025 | 59 | 94p | 88p | 1.07 | 0.71 | 0.87 | atr_ratio < 1.5; body & close-loc just below override |
| Jun 12 2025 | 60 | 148p | 91p | 1.62 | 0.63 | 0.66 | close_loc 0.66 < 0.75; grade=VALID blocks override |
| Oct 30 2025 | 160 | 90p | 57p | 1.57 | 0.40 | 0.19 | body 0.40 < 0.6; body 0.40 < 0.75 (override) |
| Dec 10 2025 | 189 | 78p | 53p | 1.47 | 0.87 | 0.93 | atr_ratio 1.47 < 1.5 by 0.03; grade=WEAK blocks override |
| Dec 22 2025 | 197 | 63p | 54p | 1.18 | 0.67 | 0.83 | atr_ratio 1.18 < 1.5; body 0.67 < 0.75 |
| Dec 23 2025 | 198 | 50p | 53p | 0.94 | 0.72 | 0.84 | ✓ Cluster path catches this |

**Pattern:** canonical HTF displacement gates (atr_mult 1.5 / body 0.6 / close 0.25 / override grade-blocked) are too strict for Daily. The `locked_baseline.yaml` tags HTF displacement as PROPOSED (forensic analysis 2026-03-15) — **never directly Olya-LOCKED**.

**Tools landed for next session:**
- Live displacement param inputs in indicator settings
- **"Daily-loose preset" toggle** — single click sets atr_mult=1.25, body=0.55, close_gate=0.35, override_body=0.65, override_close=0.15, allow_weak_override=true
- "Allow override on WEAK grade" toggle — single-lever fix for the override-blocked cases
- State table shows current effective gates + preset label

## Next session agenda

1. Walk Olya through the 5 missing-MSS examples with **Daily-loose preset ON** — verify each fires the expected MSS event
2. If preset is too loose (new false positives), narrow back via individual sliders until clean
3. Once gates dialed in, capture in `port/daily_displacement_params.yaml` with verdict
4. **Lock the Daily displacement params** — move from PROPOSED to LOCKED in `~en1gma/console/detection/locked_baseline.yaml`
5. Draft evidence note for `~en1gma/docs/reviews/` documenting:
   - Methodology rationale for the new Daily gate values
   - Pine sandbox mirror bug for PIVOTAL_003 (Mar 02) was the actual root cause; canonical algorithms were correct as-built
   - Daily displacement param tuning evidence (gate values + bar examples that drove them)
6. Port locked values back into `~en1gma/console/detection/locked_baseline.yaml` (HTF displacement section, lines 121-167)

## On the MSS lines question

Olya asked whether MSS horizontal lines were "inherited from LTF map or required for HTF map calculations". Answer: **neither**. They were purely Pine sandbox visual diagnostics. Canonical en1gma MSS emits typed fields (`broken_swing`, `origin_candle`, `displacement_payload`, `break_classification`) used downstream by `order_block` and `ote` as anchor refs — but those are data, not chart lines. Lines removed without affecting any computation.

## Locked-baseline diff (for porting)

Currently in `~en1gma/console/detection/locked_baseline.yaml`:

```yaml
displacement.params.htf:
  applies_to: [1H, 4H, 1D]
  atr_multiplier:
    locked: 1.5   # PROPOSED
  body_ratio:
    locked: 0.65  # PROPOSED  ← note: 0.65 here, mss.py uses 0.6 via disp_key
  close_gate:
    locked: 0.25  # PROPOSED
decisive_override:
  body_min: 0.75
  close_max: 0.10
  pip_floor:
    1D: 20.0
```

**Candidate Daily-locked values from this session** (pending Olya verification):

```yaml
# pending lock — after next session verifies all 5 missing-MSS cases fire
displacement.params.htf.1D_specific:
  atr_multiplier: 1.25   # was 1.5
  body_ratio: 0.55       # was 0.6
  close_gate: 0.35       # was 0.25
decisive_override:
  body_min: 0.65         # was 0.75
  close_max: 0.15        # was 0.10
  allow_weak_grade: true # NEW — extends override to WEAK grade bars
```

These tuned values are **proposed only**. Lock pending Olya screen-share verification on the 5 specific dates.
