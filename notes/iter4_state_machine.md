# Iter 4 — Map Direction state machine + 3-mode flip toggle

**Date:** 2026-05-17
**Pine file:** `pine/htf_map_v1.pine`
**Fixture:** `fixtures/2026-03-02_bearish_flip.md`

## What was built
Minimum-viable state machine:
- `var int map_direction` ∈ {-1, 0, 1}
- On any MSS event, evaluate `i_flip_mode` to decide `fire_df` and `do_flip`
- Background tint per bar (green = bull, pink = bear, none = unset)
- Persistent top-right badge showing current map_direction + mode
- DF events drawn as orange labels (DF up / DF dn)

What's INTENTIONALLY not built yet:
- `first_draw` target tracking (would gate DF only when "before target reached")
- Full `daily_state` machine (EXPANSION / PULLBACK / BALANCE / etc.)
- Range box and `current_extreme` line drawing
These belong in iter 5+ once the flip-rule mode is locked.

## The 3-mode visual contrast (the calibration question for Olya)

### Mode A — A_DELIVERY_FAILURE (current encoded en1gma rule)
- Opposite MSS → DF event fires, direction does NOT flip
- Direction stays stuck in whatever the first MSS set it to
- Screenshot: `screenshots/iter4_mode_A.png`
- Visual: chart is entirely pink (bear) with 4-5 orange "DF up" labels at each MSS up event — direction never flips back to bull

### Mode B — B_REPLACEMENT
- Opposite MSS → direction flips immediately, NO DF event
- Clean tri-color: pink (pre-history bear) → green (Jan-Feb bull) → pink (mid-late Mar bear)
- Screenshot: `screenshots/iter4_mode_B.png`
- Visual: regime changes are perfectly readable, no DF noise

### Mode C — C_HYBRID (Olya's expected behavior per fixture)
- Opposite MSS → direction flips AND DF event fires
- Same tri-color regime tinting as Mode B, plus orange DF labels at the flip bars
- Screenshot: `screenshots/iter4_mode_C.png`
- Visual: best of both — clear regime, plus the DF event marker Olya wants

## Open observation for Olya review
The Pine implementation flips direction (in Mode B/C) at the FIRST MSS dn event after the bull phase. Visually this appears in mid-late March, NOT exactly on 2026-03-02. Three possibilities:
1. The Pine swing-low pool didn't contain a swing whose break happened ON 2 Mar — the Mar 2 candle is a STRONG bear displacement, but it doesn't *close below* any pre-existing SL until later
2. Olya's "MSS turns bearish on 2 Mar" may be a looser term ("regime shifted") rather than the strict primitive definition
3. There's a sub-SL the 15-pip filter excludes that would have been broken on Mar 2

This is the precise calibration question to put in front of Olya: does the bear flip belong at 2026-03-02 specifically, or wherever the first valid SL break occurs after the bull phase ends?

## Next iterations (post-Olya lock)
- Lock the chosen mode (A / B / C) in `port/direction_flip_rule.yaml`
- Add `first_draw` tracking so DF events fire only when the bar happens *before* the prior range's First Draw was reached (the current methodology nuance Mode A theoretically gates on)
- Add `daily_state` machine (EXPANSION / PULLBACK / BALANCE / APPROACHING_TARGET / REASSESSMENT)
- Add range box + `current_extreme` line draw
