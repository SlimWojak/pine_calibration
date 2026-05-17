# Calibration scorecard

Living document. Each fixture is a row. Status flips from TODO → PARTIAL → PASS as the Pine indicator earns visual parity with Olya's pre-flagged behavior.

When all rows are PASS and the corresponding `port/*.yaml` rule contracts are signed off by Olya, the slice is ready for Python port-back.

| Fixture | What should fire (Olya) | What Pine fires | Screenshot | Status |
|---|---|---|---|---|
| `2026-03-02_bearish_flip` | Direction flip + DF event + bearish range birth + active_extreme_low from 3 Mar | **Mode A**: no flip, DF up only (stuck bear). **Mode B**: flip on first SL break after bull phase (mid-Mar, not 2 Mar), no DF. **Mode C**: flip + DF dn on same bar. | `iter4_mode_A/B/C.png`, `olya_session_*.png` | PARTIAL — **root cause found**: Daily swing params (N=2 h=15) too strict, missing obvious SWH/SWL. Lowering filter moves bear flip from Mar 12 → Mar 2-3 area. Currently dialing N+h with Olya (paused at N=1 h=3, pending verdict). |
| `2025-12-10_range_birth` | (fill from review pack) | — | — | TODO |
| `2026-01-22_range_succession` | (fill from review pack) | — | — | TODO |
| `2025-12-24_target_reassess` | (fill from review pack) | — | — | TODO |
| `h4_timing_visibility` | H4 transitions visible at pivotal Daily moments | — | — | TODO |

## Mode selection log (for 3-way toggles)

| Rule | Modes considered | Olya-selected mode | Locked in `port/` |
|---|---|---|---|
| Direction-flip rule (Mar 2) | A=Delivery Failure / B=Replacement / C=Hybrid (built + visualized 2026-05-17) | C_HYBRID inferred from session direction; not explicitly ruled yet — revisit after swings locked | `port/direction_flip_rule.yaml` (not yet written) |
| Daily swing params (NEW — surfaced during 2026-05-17 session) | en1gma default N=2 h=15 too strict — Olya wants N=? h=? to catch obvious SWH/SWL she listed | Iterating: baseline → N2 h5 → N2 h3 → **N1 h3 (current, pending verdict)** | `port/daily_swing_params.yaml` (to be written) |
