# Iter 1 — Daily swing detection

**Date:** 2026-05-17
**Pine file:** `pine/htf_map_v1.pine` (commit TBD)
**Chart:** FX:EURUSD Daily, visible range Jan 1 – Apr 30 2026

## What was built
- Daily `swing_points` mirroring locked params from `en1gma/console/detection/locked_baseline.yaml`:
  - `N=2` (fractal-2 pivots via `ta.pivothigh(2,2)` / `ta.pivotlow(2,2)`)
  - `height_filter_pips=15.0` — minimum vertical distance from the swing wick to the highest (SH) / lowest (SL) neighbor wick in the 2N-window
- Visual debug: red cross at SH (offset back by N bars to the true pivot bar); green cross at SL same way.

## What fires on Jan-Apr 2026 EUR/USD Daily
- ~5 SH crosses at the major peaks (mid-Jan ~1.18 peak, late-Jan retest, mid-Feb, late-Feb just before the Mar 2 flip, and one later)
- 2 SL crosses at the mid-March bottom consolidation
- The Late-Feb SH just before the big bearish drop is the swing that should get broken on Mar 2 → this is what `daily_mss` will need to consume next iteration.

## Open questions for visual sign-off (G)
- Do the swing locations match what Olya would call "structurally meaningful pivots" on this window?
- Are there missed swings I'd expect to see (false negatives from the 15-pip filter)?
- Are there spurious swings I'd expect to be filtered out (false positives)?

Screenshot: `screenshots/iter1_swings_jan_apr_2026.png`
