# Olya session state — PAUSED 2026-05-17

**Status:** Olya stepped out mid-session. Resuming in ~1hr.
**Goal of session:** Calibrate Daily swing primitive params with Olya, then move to the Mar 2 bearish flip mode adjudication.

## Where we are when resuming

**Pine file `pine/htf_map_v1.pine` defaults (currently loaded on chart):**
- `i_swing_n = 1`
- `i_swing_height_pips = 3.0`
- `i_flip_mode = "C_HYBRID"`
- Isolation toggles: ONLY `i_show_swings`, `i_show_pivotal`, `i_show_table` ON. Everything else OFF.

**TradingView Desktop state:**
- Chart: FX:EURUSD, 1D
- Visible range: roughly Dec 2025 → May 2026
- Indicator: HTF MAP V1 — calibration sandbox (entity_id rotates per re-inject; use `chart_get_state` to fetch on resume)

## What Olya has ruled so far (this session)

### Mode adjudication (Q1 from walkthrough)
- Inferred-but-not-explicit: Mode C_HYBRID looks correct (flip + DF event on opposite MSS), but Olya did NOT explicitly rule yet. The conversation pivoted to swing-primitive calibration before we landed on this.
- **Need to revisit Q1 explicitly on resume** before calibrating downstream primitives, OR park it until swings are locked (since swing changes will shift MSS/DF event positions).

### Swing primitive calibration (the meat of session so far)
The HTF MAP's Daily swing params `(N=2, height_filter_pips=15.0)` in `en1gma/console/detection/locked_baseline.yaml` are tagged **PROPOSED** (LTF-confirmed only). Olya saw obvious SWH/SWL being missed at those defaults — caused the Mar 2 bear flip to fail to fire and instead land on Mar 12.

**Iterations run live with Olya watching:**

| Step | N | height (pips) | Olya verdict | Notes |
|---|---|---|---|---|
| Baseline | 2 | 15 | "Missing many obvious swings" | Bear flip landed Mar 12, not Mar 2. en1gma default. |
| Iter 1 | 2 | 5 | "Lots more SWH/SWL now, but still missing some: 10 Feb SWH, 19 Feb SWL, 30 Mar SWL" | Bear flip moved to Mar 2-3 area. Big improvement. |
| Iter 2 | 2 | 3 | "30 Mar SWL caught. Still missing 10 Feb SWH, 19 Feb SWL" | Diagnostic: those two are likely 3-bar fractals → N=2 rejects them regardless of height filter. |
| Iter 3 (CURRENT — PENDING) | 1 | 3 | **NOT YET VERDICTED — Olya stepped out** | N=1 catches 3-bar fractals. Olya needs to check (a) does this catch 10 Feb / 19 Feb (b) is it now too noisy. |

**Specific swings Olya wants captured** (from her listing during call):
- Mon 19 Jan SWL
- 6 Feb low
- 10 Feb high ← still missing at N=2 h=3
- 19 Feb low ← still missing at N=2 h=3
- 27 Feb SWH (would mark start of bearish expansion range)
- 13 Mar low
- 23 Mar high
- 30 Mar low ← caught at N=2 h=3
- 6 Apr low

## What to do on resume

### Immediate (next 5-10 min with Olya)

1. **Get her verdict on N=1, h=3** — already loaded on chart, just bring her back to the same swing-only view:
   - Are 10 Feb SWH and 19 Feb SWL caught now?
   - Are any spurious crosses appearing (noise) that she'd want filtered out?
2. **If N=1 h=3 is right** → lock these as the Daily swing params for the calibration session. Move to next primitive.
3. **If still missing** → try N=1 h=1, then N=1 h=0 (pure fractal-N=1, no height filter). She might also point to specific bars to investigate.
4. **If too noisy** → raise h back up (4, 5, 7) keeping N=1.

### Then (the rest of session per walkthrough doc)

5. **Lock the direction-flip mode** — flip back on `i_show_direction` + `i_show_df_events`, ask explicit Q1 from walkthrough (A/B/C).
6. **Move to displacement primitive** — flip on `i_show_displacement`, ask Olya if yellow diamonds (STRONG) and aqua circles (VALID) match her mental model of decisive Daily bars.
7. **Move to MSS** — flip on `i_show_mss`, walk Mar 2 area in bar replay (`replay_start date=2026-02-25`, step bar by bar through Mar 6).
8. **Capture rulings** — write `port/direction_flip_rule.yaml` AND `port/daily_swing_params.yaml` AND update `scorecard.md`.

### Workflow gotcha to remember
The user requested a primitive-isolation workflow: tune one primitive at a time with all others hidden, then layer back on. Toggle inputs already exist in the indicator under group "Layer toggles (isolate primitives)" — right-click indicator on chart → Settings → expand that group to flip checkboxes.

## Calibration findings to bring back to en1gma (do NOT update en1gma in this session)

These need a separate G-ratified lane in `~en1gma`:
- Daily `swing_points` params drift: `en1gma/console/detection/locked_baseline.yaml` has Daily `N=2, height_filter_pips=15.0` (PROPOSED) — visual calibration on FX:EURUSD Dec25-Apr26 with Olya indicates **TRUE locked values are much more permissive**. Likely candidate: N=1 (or N=2 with much lower h). Final value pending Olya verdict from N=1 h=3 iteration.
- The `DETECTION_PRIMITIVES_INDEX.md` already notes Daily params PROPOSED pending Olya — this session is the Olya touchpoint. Calibration finding to be written into a formal en1gma evidence note before any code change.

## Screenshots captured this session
- `screenshots/olya_session_N2_h15_baseline.png` — locked-baseline default, bear flip at Mar 12
- `screenshots/olya_session_N2_h5.png` — first improvement, bear flip moved to Mar 2-3 area
- `screenshots/olya_session_N2_h3.png` — Mar 2-3 flip + more swings, but 10/19 Feb still missing
- `screenshots/olya_session_isolated_N2_h3.png` — swing-only isolation view, clean canvas
- `screenshots/olya_session_state_N1_h3_pending_verdict.png` — pending (CDP blipped, may need recapture on resume)

## Pickup quick-start

```
1. Open TradingView Desktop, ensure FX:EURUSD 1D, indicator loaded
2. If indicator not on chart: pine_open "HTF MAP V1", then chart_manage_indicator add via ui_evaluate
3. Confirm inputs in state-table: should read "N=1, h=3.0p" with Mode C_HYBRID
4. Bring Olya back, point at chart, ask "does N=1 h=3 catch 10 Feb SWH and 19 Feb SWL?"
5. Continue per "What to do on resume" above
```
