# 2026-03-02 — Mar bearish flip (PIVOTAL_003)

## Source
- Olya review pack: `en1gma/docs/reviews/MAP_V1_GATE_2_PIVOTAL_PIVOTAL_003_MAR_BEARISH_FLIP_OUTPUT_FOR_OLYA_2026_05_16.md`
- Private intermediate: `en1gma/docs/reviews/MAP_V1_GATE_2_PRIVATE_INTERMEDIATE_PIVOTAL_003_MAR_BEARISH_FLIP_2026_05_16.yaml`
- Raw event id: `2026_03_02_bearish_flip`
- Window: 2026-03-02 to 2026-03-03

## What the chart should show (Olya verbatim)
> 2nd of March 2026
>  - Broke bullish order flow
>  - MSS turns bearish on daily on 2nd of March
>  - Low of the bearish range was 3rd of March

## Expected map cells (per Olya pre-flag)
- Map Direction: transition bullish → bearish on 2 Mar
- Delivery Failure: candidate event fires on 2 Mar
- Active Daily Dealing Range: bearish range birth 2-3 Mar
- Current Extreme: bearish active_extreme_low tracking from 3 Mar

## Current Python map output (for contrast — what to fix)
- daily_state: `Reassessment`
- h4_timing: `no_transition`
- active_daily_range: `None` (extension active_extreme 1.15303)
- current_extreme: `None` 1.15303
- daily_location_daily_pda_respect_control: `premium`
- first_draw_main_dol: `PIVOTAL_003_MAR_BEARISH_FLIP_active_range_extreme None`
- delivery_failure: `false`
- map_direction_dependency: `bearish` *(direction itself does flip; downstream effects don't)*

## What the Pine slice must surface
- A visible Map Direction label that flips bullish → bearish on the 2 Mar Daily candle
- A clearly-drawn Delivery Failure event marker at the flip bar
- A new dealing-range box that births on 2-3 Mar, with the low wick of 3 Mar as the active_extreme
- A live readout (table or label) of daily_state through the flip

## Open methodology question to adjudicate with Olya
**Direction-flip rule:** when an opposite-direction Daily MSS+displacement fires *before* the prior range's First Draw is hit, should the map:
- **A — Delivery Failure mode** (current encoded rule): emit DF event, daily_state → REASSESSMENT, no immediate flip; flip waits for next valid MSS
- **B — Replacement mode**: immediate Map Direction flip, new range birth, no DF event
- **C — Hybrid**: DF event *and* direction flip simultaneously (current observed `mss_dependency=bearish + delivery_failure=false` looks like a broken mix of A and B)

The Pine indicator will expose this as a 3-way toggle so Olya can pick the mode that matches her chart-view on Mar 2.

## Calibration acceptance (when this card passes)
- All four "Expected map cells" render correctly on the Pine indicator for the chosen mode
- Screenshot captured at `screenshots/2026-03-02_<mode>_flip.png`
- Olya signs off on which mode is the locked rule → captured in `port/direction_flip_rule.yaml`

## Olya session log (in flight — 2026-05-17, paused)
**Root cause of the Mar 12 / Mar 2 mismatch:** the underlying `swing_points` Daily params are too strict at the en1gma locked-baseline defaults (`N=2, height_filter_pips=15.0`, which are tagged PROPOSED for HTF — never Olya-confirmed on Daily). Many "obvious" SWH/SWL Olya can see on the chart are filtered out, which propagates: missing SLs → no bearish MSS on Mar 2 → no flip → Mar 12 finally clears a later SL.

**Calibration so far (defaults applied in-Pine, not yet in en1gma):**
| N | h (pips) | Bear flip lands | Olya |
|---|---|---|---|
| 2 | 15 | Mar 12 | "Missing many obvious swings" |
| 2 | 5  | Mar 2-3 area | "Closer, still missing some" |
| 2 | 3  | Mar 2-3 area | "30 Mar SWL caught. 10 Feb SWH, 19 Feb SWL still missing" |
| 1 | 3  | TBD | **PENDING VERDICT** — Olya stepped out before verifying |

See `notes/olya_session_state.md` for full pickup context.
