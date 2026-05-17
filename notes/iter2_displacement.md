# Iter 2 — Daily displacement

**Date:** 2026-05-17
**Pine file:** `pine/htf_map_v1.pine`

## What was built
Daily `displacement` mirroring locked params from `locked_baseline.yaml`:
- `htf_atr_multiplier=1.50`, `htf_body_ratio=0.65`, `htf_close_gate=0.25`
- `decisive_override`: body_min=0.75, close_max=0.10 → STRONG
- `cluster_2`: net_efficiency_min=0.65 AND combined range >= ATR*1.5
- `combination_mode=AND` (ATR AND body AND close-gate, not OR)

## Three qualification paths (mirrors python detector)
1. `single_override` (decisive) → STRONG
2. `single_atr` (all-AND single bar) → VALID
3. `cluster_2` (2 same-direction bars, both significance + efficiency) → VALID
   - Gate: cluster only fires when single-bar paths did NOT fire on either bar of the pair (avoids double-counting)

## False-positive incident worth remembering
First implementation had cluster_2 only checking `net_efficiency >= 0.65` (no ATR floor). On a trending Daily, almost every 2-bar pair qualified — chart was a wall of bgcolor highlights. Fix:
1. Require combined range >= ATR*1.5 on cluster path (matches single-bar significance gate)
2. Suppress cluster firing when either bar already fired single-bar (so cluster is genuinely a 2-bar *fallback*, not additive)

## Visual
- STRONG = yellow diamond ABOVE bar (bear) or BELOW bar (bull) — markers point into the move
- VALID  = aqua circle, same convention
- Swing crosses unchanged

## What fires on Jan-Apr 2026 EUR/USD Daily
- ~7 STRONG diamonds across the window (the actual decisive thrust days)
- ~10 VALID circles (single_atr or cluster moves)
- **Mar 2 region: yellow diamond ABOVE the big bearish candle** — STRONG bear displacement at the precise bar Olya pre-flagged. This is the input `daily_mss` consumes next.
- The SH that gets broken on Mar 2 is visible as a red cross at the immediately-prior swing high.

Screenshot: `screenshots/iter2_displacement.png`
