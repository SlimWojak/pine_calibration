# Iter 3 — Daily MSS

**Date:** 2026-05-17
**Pine file:** `pine/htf_map_v1.pine`

## What was built
Daily `mss` mirroring locked params:
- `htf_displacement_required=true` (gated by displacement output from iter 2)
- `htf_close_beyond_swing=true` (`close > sh` for bull, `close < sl` for bear)
- `htf_confirmation_window_bars=1` (displacement on break bar OR prior bar)
- `swing_consumption=true` (broken swing removed from candidate pool)

## Data structure
Two persistent `var array<float>` buffers (one for unconsumed SH prices, one for SLs) plus parallel `var array<int>` arrays for the original swing bar indices (needed to draw line from swing → break).

## Visual
- Green horizontal line + "MSS up" label = bullish MSS (broken SH)
- Red horizontal line + "MSS dn" label = bearish MSS (broken SL)
- Line spans from swing bar to break bar at the swing price

## What fires on Jan-Apr 2026 EUR/USD Daily
- ~5 MSS up events clustered during Jan-Feb bull phase
- 2 MSS dn events during mid-late March bear phase
- Each event consumes exactly one swing (one line, one label per MSS)

## Mar 2 verification status
Visual shows the bear-phase MSS dn events occurring in mid-late March, AFTER the Mar 2 flip candle. Two possibilities:
1. The Mar 2 candle was a STRONG bear displacement but didn't close below any pre-existing SL → no MSS dn event there
2. Olya's "MSS turns bearish on 2 Mar" may refer to a different anchor swing than what my Pine pool contains

Will verify exact dates via bar-replay once state machine is in place.

Screenshot: `screenshots/iter3_mss.png`
