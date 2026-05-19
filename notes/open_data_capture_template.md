# Open Data Capture — 2026-05-19 Olya Session

Deferred anchors from ~en1gma/docs/reviews/MAP_V1_GATE_2_OLYA_PERDAY_SESSION_STOCKTAKE_2026_05_18.md §3 `deferred_anchors_to_capture_next`. Fill if Olya is available during the screen-share; mark `STILL_OPEN` if not addressed so the next-next session knows.

## 1. Jan 22 FVG range

**Context:** Bullish FVG that gets tested on Feb 2, Feb 6, Feb 19 (per Olya), then broken on Mar 2 (V013).

**Needed:** the FVG's specific price range (top / bottom).

**Source verdict:** V013 (Mar 2 flip evidence — FVG-break portion §6 gated).

**Why deferred:** §6 OB/BPR/IFVG/composite PDA standing stop in en1gma. Pine capture is observation only — does NOT authorize §6 work in en1gma.

| Field | Value |
|---|---|
| FVG top price | TBD |
| FVG bottom price | TBD |
| Detected on bar (date) | TBD |
| Olya verbatim | TBD |
| Status | OPEN / CAPTURED / STILL_OPEN |

## 2. Weekly low broken on Mar 13

**Context:** Mar 13 is the full bullish→bearish trend pivot (V015). At Daily scale it's MSS+Displacement; at Weekly scale it breaks a prior Weekly low.

**Needed:** the specific Weekly low price level Mar 13 broke.

**Source verdict:** V015 (additional_semantics field in stocktake §3).

| Field | Value |
|---|---|
| Weekly low price | TBD |
| Week-ending date of the broken low | TBD |
| Olya verbatim | TBD |
| Status | OPEN / CAPTURED / STILL_OPEN |

## 3. Mar 10 bearish range high — specific price

**Context:** V014 — Mar 10-16 cascade. Olya named Mar 10 as a range_origin anchor (V001 anchor #4) but specific price for the bearish range high was not captured 2026-05-18.

**Needed:** the range high price on Mar 10 (or whichever bar in the Mar 10-16 cascade Olya identifies as the controlling range high).

**Source verdict:** V014.

| Field | Value |
|---|---|
| Bearish range high price | TBD |
| Anchor date | TBD |
| Olya verbatim | TBD |
| Status | OPEN / CAPTURED / STILL_OPEN |

## 4. Jan 20 displacement bar parameters

**Context:** Jan 20 is a missing-MSS calibration case (V011). Daily-loose preset should fire MSS by breaking Jan 12 SWH @ 1.16985. To verify the preset values are correct for this bar, capture the bar's displacement-relevant numerics.

**Pine can compute these directly** — no Olya needed for the numbers. Olya needed for: confirm Daily-loose preset fires MSS visually on this bar.

| Field | Value (Pine-computed) |
|---|---|
| Bar range (high - low, pips) | TBD |
| ATR (14, prev bar) | TBD |
| atr_ratio (range / ATR) | TBD |
| body | TBD |
| close-loc | TBD |
| Daily-loose preset fires? | TBD (Olya visual confirmation) |
| Status | OPEN / CAPTURED |

## Cross-reference

After session, copy CAPTURED rows into:
- Jan 22 FVG, Weekly low Mar 13, Mar 10 range high → notes/preflight_results_2026_05_19.md follow-up section
- Jan 20 displacement params → port/daily_displacement_params.yaml `verification.mss_fire_test` Jan 20 row pine_pre_flight field
