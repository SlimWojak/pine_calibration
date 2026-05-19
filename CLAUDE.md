# CLAUDE.md — pine_calibration

## STATUS: PARKED 2026-05-19

This repo is no longer the primary HTF Map V1 visual calibration surface. Pivot anchor: `~/en1gma/docs/reviews/HTF_CALIBRATION_PIVOT_PINE_TO_RESEARCH_ACCELERATOR_2026_05_19.md`.

Primary HTF calibration is now `~/research_accelerator` (canon-by-inheritance harness; same tool that locked LTF primitives with Olya). See `~/research_accelerator/CLAUDE.md`.

What this means for fresh agents:

- Do NOT extend the Pine indicator further. No new primitives. No GOAL mode dispatch on Pine extension. The PINE_PRIMITIVE_EQUALISATION_BRIEF.md at `notes/` is OBSOLETE; do not act on it.
- Pine indicator (`pine/htf_map_v1.pine`, 565 lines, 3 primitives canon-mirrored as of fix #11) is preserved as a stable sanity-check artifact. Useful when Olya has TV open and wants ad-hoc live verification on her trading terminal.
- Olya rulings captured here (May 17 + May 19 sessions) have been migrated into the pivot brief section 5 for V005 sprint absorption. Source files (`port/*.yaml`, `notes/olya_session_*.md`, `fixtures/*`) remain in repo as record.
- If you need to read a specific orphan ruling, see pivot brief section 5.1 through 5.6 for index plus citations back to source files in this repo.

## Original Purpose (HISTORICAL — pre-pivot)

Sandbox for calibrating the HTF MAP algorithm in Pine Script v6. Output of this repo is a **locked algorithm definition** that gets ported to `~en1gma` Python. This is not production code — optimize for iteration speed and visual clarity, not robustness.

For HTF Map V1 work this purpose drifted from "algorithm exploration with port-back" to "strict canon mirror of already-locked algorithms" — a different shape of work with a permanent maintenance tax. The pivot to `~research_accelerator` removes that tax. The original purpose still applies for any future LTF or experimental algorithm slice that genuinely needs Pine v6 prototyping; it does NOT apply for HTF Map V1.

## Hard rules

- **Pine Script v6 only.** Use the `pinescript-docs` MCP to verify function signatures before writing. Training-data syntax often drifts into v5.
- **Locked params are CODE-OF-RECORD.** Every constant in `pine/htf_map_v1.pine` must match `en1gma/console/detection/locked_baseline.yaml` verbatim. Per `en1gma/docs/canonical/DETECTION_PRIMITIVES_INDEX.md`: **CODE WINS**. Do not retune in this lab — retuning happens in `~en1gma`.
- **No verbose docs.** The reason this repo exists is to escape doc-heavy cycles. Notes in `notes/` should be terse: what was tried, what fired, what we learned. No multi-page review packs.
- **Don't replicate `~en1gma` complexity here.** If a layer isn't load-bearing for the HTF MAP visual loop, leave it out.
- **Signal-diff is the port-back contract.** When Pine → Python, both must emit identical fire timestamps on the same input. Any divergence is a bug in exactly one of them.

## MCP usage

- `tradingview.*` — chart reading, Pine inject/compile, bar replay, screenshots, UI control
- `pinescript-docs.pinescript_reference` — function/variable lookup (always do this before writing)
- `pinescript-docs.pinescript_review` — code review before considering a slice done

## Workflow per iteration

1. Look up Pine functions via `pinescript-docs`
2. Write/edit Pine in `pine/htf_map_v1.pine` — mirror locked params verbatim
3. `pine_set_source` → `pine_smart_compile` → check errors
4. **Add to chart** is a manual step — see "Learnings" below
5. Adjust visible range (`chart_set_visible_range`) or run bar replay (`replay_start` / `replay_step` / `replay_stop`) to inspect the iteration's target window
6. Screenshot to `screenshots/iterN_<slug>.png` + annotate in `notes/iterN_<slug>.md`
7. Update `scorecard.md`
8. When Olya rules: capture locked rule in `port/<rule>.yaml` from `_template.yaml`, mark scorecard PASS, then port to Python in `~en1gma`

## Learnings (don't relearn)

- **cluster_2 displacement needs an ATR floor + single-bar-suppression.** First iteration made cluster_2 fire on any 2 same-direction bars meeting `net_efficiency >= 0.65` — chart was a wall of highlights in trending markets. Fix: also require combined range >= ATR*1.5, AND suppress cluster when either bar already qualified single-bar. See `notes/iter2_displacement.md`.
- **`pine_smart_compile` does NOT auto-add freshly-saved scripts to the chart.** It clicks the editor's Save button only. To add: find the editor button with `title="Add to chart"` (the `aria-label` is unset) and click via `ui_evaluate` — see the snippet pattern in `notes/iter4_state_machine.md` commit history.
- **Edit tool string-matching drifts when `pine_set_source` overwrites have differing whitespace.** When iterating fast, just `Write` the whole file fresh instead of fighting whitespace-sensitive Edits.

## Related repos

- `~en1gma` — source of truth for the HTF MAP spec. Canonical docs:
  - `docs/canonical/MAP_SPATIAL_PRIMER_v1.md` — methodology
  - `docs/canonical/DETECTION_PRIMITIVES_INDEX.md` — primitives index (CODE WINS rule)
  - `docs/reviews/PHASE_5_C2_DAILY_LED_HTF_MAP_V1_METHODOLOGY_BASELINE_RATIFIED_2026_05_05.md` — baseline
  - `docs/reviews/MAP_V1_GATE_2_OLYA_HUMAN_REVIEW_PACK_2026_05_16.md` — Gate 2 pack
  - `docs/reviews/MAP_V1_GATE_2_COUNTERFACTUAL_LAB_EVIDENCE_NOTE_2026_05_17.md` — counterfactual evidence
- `~research_accelerator` — PRIMARY HTF Map V1 visual calibration surface (post-pivot 2026-05-19); was LTF visual-tuning precedent pre-pivot. See `~/research_accelerator/CLAUDE.md`.
