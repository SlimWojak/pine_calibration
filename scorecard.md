# Calibration scorecard

Living document. Each fixture is a row. Status flips from TODO → PARTIAL → PASS as the Pine indicator earns visual parity with Olya's pre-flagged behavior.

When all rows are PASS and the corresponding `port/*.yaml` rule contracts are signed off by Olya, the slice is ready for Python port-back.

| Fixture | What should fire (Olya) | What Pine fires | Screenshot | Status |
|---|---|---|---|---|
| `2026-03-02_bearish_flip` | Direction flip + DF event + bearish range birth + active_extreme_low from 3 Mar | — | — | TODO |
| `2025-12-10_range_birth` | (fill from review pack) | — | — | TODO |
| `2026-01-22_range_succession` | (fill from review pack) | — | — | TODO |
| `2025-12-24_target_reassess` | (fill from review pack) | — | — | TODO |
| `h4_timing_visibility` | H4 transitions visible at pivotal Daily moments | — | — | TODO |

## Mode selection log (for 3-way toggles)

| Rule | Modes considered | Olya-selected mode | Locked in `port/` |
|---|---|---|---|
| Direction-flip rule (Mar 2) | A=Delivery Failure / B=Replacement / C=Hybrid | TBD | `port/direction_flip_rule.yaml` (not yet written) |
