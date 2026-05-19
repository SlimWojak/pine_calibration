# Calibration scorecard

Living document. Each fixture is a row. Status flips from TODO → PARTIAL → PASS as the Pine indicator earns visual parity with Olya's pre-flagged behavior.

When all rows are PASS and the corresponding `port/*.yaml` rule contracts are signed off by Olya, the slice is ready for Python port-back.

| Fixture | What should fire (Olya) | What Pine fires | Screenshot | Status |
|---|---|---|---|---|
| `2026-03-02_bearish_flip` | Direction flip + DF event + bearish range birth + active_extreme_low from 3 Mar | After 4 canon-mirror fixes (swing_points, MSS most-recent-only, MSS suppression, displacement grades): MSS dn correctly lands on Mar 02 against Feb 19 SWL @ 1.17418. Olya signed off on Mar 02. | `olya_fix1_mar02_visible.png`, `olya_fix3_*`, `olya_fix5_legend_simplified.png` | **PASS** (algo correct, awaiting Daily disp gates lock) |
| `2025-12-10_range_birth` | MSS up at Dec 10 (clear break per Olya) | Currently algorithm consumes silently — atr_ratio 1.47 < 1.5 by 0.03; grade=WEAK blocks override | — | **OPEN** — needs Daily-loose preset or "allow override on WEAK" |
| `2025-06-11_clear_mss` | Clear MSS at Jun 11 | atr_ratio 1.07 < 1.5; body 0.71 & close 0.87 just below override gates | — | **OPEN** — needs looser gates |
| `2025-06-12_clear_mss` | Clear MSS at Jun 12 | close_loc 0.66 < 0.75; grade=VALID blocks override | — | **OPEN** — needs looser gates |
| `2025-10-30_clear_mss` | Bear MSS at Oct 30 | body 0.40 < 0.6 AND fail; body 0.40 < 0.75 override fail | — | **OPEN** — needs looser body gate |
| `2025-12-22_clear_mss` | Bull MSS at Dec 22 | atr_ratio 1.18 < 1.5; body 0.67 < 0.75 | — | **OPEN** — needs looser gates |
| `2025-12-23_clear_mss` | Bull MSS at Dec 23 | ✓ Cluster path catches | — | PASS |
| `2026-01-22_range_succession` | (fill from review pack) | — | — | TODO |
| `2025-12-24_target_reassess` | (fill from review pack) | — | — | TODO |
| `h4_timing_visibility` | H4 transitions visible at pivotal Daily moments | — | — | TODO |

## Mode selection log (for 3-way toggles)

| Rule | Modes considered | Olya-selected mode | Locked in `port/` |
|---|---|---|---|
| Direction-flip rule (Mar 2) | A=Delivery Failure / B=Replacement / C=Hybrid (built + visualized 2026-05-17) | C_HYBRID provisional (matches Mar 02 ruling); explicit Q1 confirmation captured | `port/direction_flip_rule.yaml` (to be written) |
| Daily swing params | en1gma `N=2 h=15` (PROPOSED) | **LOCKED 2026-05-19 by Olya.** N=2, h_display=15p. 5 SURFACE anchors PASS (Dec 9, Dec 16, Jan 12 linchpin, Jan 19, Jan 27); 7 regression anchors HELD; N=1 experiment ruled out (too much noise for 3 fringe dates Aug 5 / Jan 22 / Feb 4) | `port/daily_swing_params.yaml` LOCKED |
| Daily displacement gates | Canon HTF `atr=1.5 body=0.6 close=0.25 override-no-grade` (PROPOSED, never Olya-LOCKED) | Awaiting Olya verification of looser values via Daily-loose preset on missing-MSS cases | `port/daily_displacement_params.yaml` (pending) |

## Session log
- 2026-05-17 morning: Daily swing params iteration (paused mid-session at N=1 h=3, pending Olya verdict)
- 2026-05-17 afternoon: **Canon-audit pivot**. Found 4 Pine ≠ canonical algorithm bugs. Landed fixes. Olya signed off on Mar 02. New issue surfaced: Daily displacement gates too strict for Olya's missing-MSS examples.
- 2026-05-19: **Phase 0 SWH/SWL primitive LOCKED** at N=2 by Olya. **MSS fix #6** landed (canon-mirror broken-flag pattern, fixes cascading-fires bug). **Late-fire parked** as swing-detection-granularity tradeoff. **5 new Phase 1 tuning anchors** identified (May 3/14, Jul 3/11/23 2024 — no displacement detection). Full session captured in `notes/olya_session_2026_05_19.md`.
- Next session: Phase 1 displacement gate tuning with fixed Pine. Walk all anchors with Daily-loose preset, tune until clean, lock `port/daily_displacement_params.yaml`. Handoff to V005 en1gma sprint.
