# CLAUDE.md — pine_calibration

## Purpose

Sandbox for calibrating the HTF MAP algorithm in Pine Script v6. Output of this repo is a locked algorithm definition that gets ported to `~en1gma` Python. **This is not production code.** Optimize for iteration speed and clarity, not robustness.

## Hard rules

- **Pine Script v6 only.** Use the `pinescript-docs` MCP to verify function signatures before writing — do not rely on training data, which often drifts into v5.
- **No verbose docs.** The reason this repo exists is to escape doc-heavy review cycles. Notes in `notes/` should be terse: what was tried, what fired, what we learned. No multi-page review packs.
- **Don't replicate `~en1gma` complexity here.** If a layer (regime detection, multi-asset, infra) isn't load-bearing for the HTF MAP visual loop, leave it out.
- **Signal-diff is the contract.** When porting back, the Python and Pine implementations must produce identical fire timestamps on the same input. Any divergence is a bug — in one of them.

## MCP usage

- `tradingview.*` tools — for chart reading, Pine inject/compile, bar replay, screenshots
- `pinescript-docs.pinescript_reference` — function/variable lookup
- `pinescript-docs.pinescript_review` — code review before considering a file done

## Workflow per iteration

1. Look up any Pine functions I'm unsure about via `pinescript-docs`
2. Write/edit Pine in `pine/`
3. Inject + compile via `tradingview` MCP, read errors, fix
4. Apply to chart, step through bars (replay mode)
5. Screenshot key moments, note in `notes/` what fired correctly vs incorrectly
6. Tune and repeat

## Related repos

- `~en1gma` — source of truth for the HTF MAP spec. Canonical docs:
  - `docs/canonical/MAP_SPATIAL_PRIMER_v1.md`
  - `docs/reviews/PHASE_5_C2_DAILY_LED_HTF_MAP_V1_METHODOLOGY_BASELINE_RATIFIED_2026_05_05.md`
  - `docs/reviews/MAP_V1_GATE_2_OLYA_HUMAN_REVIEW_PACK_2026_05_16.md`
  - `docs/reviews/MAP_V1_GATE_2_COUNTERFACTUAL_LAB_EVIDENCE_NOTE_2026_05_17.md`
