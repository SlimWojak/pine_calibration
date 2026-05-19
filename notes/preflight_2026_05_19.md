# Pre-flight — 2026-05-19 (before Olya Daily-loose verification session)

## Purpose

Verify the Daily-loose preset toggle works mechanically, capture canonical/loose comparison
screenshots, and walk into the live screen-share with evidence rather than discovery.
Anchor list per [[port/daily_displacement_params.yaml]] verification matrix:
7 MSS_FIRE_TEST + 7 SURFACE_TEST + 8 REGRESSION = 22 bar/date anchors total.

Authoritative source: ~en1gma/docs/reviews/MAP_V1_GATE_2_OLYA_PERDAY_SESSION_STOCKTAKE_2026_05_18.md §3

## Session-state checks (pre-flight order)

1. **Repo state** — iter-6 work committed locally (commit 865d641). No outstanding WIP. ✅
2. **MCP health** — tradingview-mcp + pinescript-docs both responsive; CDP connected to FX:EURUSD 1D chart. ✅
3. **Indicator on chart** — "HTF MAP V1 — calibration sandbox" loaded (entity_id `mVF5sg`). ✅
4. **Chart input state** — canonical defaults per state table: `N=2, h>=15p disp; atr>=1.5, body>=0.6, cg<=0.25 [canon]`. No drift from session-end. ✅
5. **Daily-loose preset toggle mechanics** — verified working (see §Procedure below). ✅

## Procedure used (and gotchas)

The indicator's Daily-loose preset is `i_loose_preset` in the Pine source (line 48), the 7th
declared input (zero-indexed = `in_6`).

**Gotcha:** `indicator_set_inputs` does NOT accept the Pine variable name as the key. The
key must be the zero-indexed input ID:

| Attempt | Result |
|---|---|
| `{"i_loose_preset": true}` | `updated_inputs: {}` (no-op, no error) |
| `{"in_6": true}` | `updated_inputs: {"in_6": true}` ✓ State table → `[LOOSE]` |

For during-session control of any of the live displacement sliders, the input order is:
- `in_0` i_swing_n
- `in_1` i_min_height_pips
- `in_2` i_swing_date_labels
- `in_3` i_flip_mode
- `in_4` i_atr_length
- `in_5` i_use_suppression
- **`in_6` i_loose_preset** ← toggle
- `in_7` i_atr_mult
- `in_8` i_body_ratio
- `in_9` i_close_gate
- `in_10` i_override_body
- `in_11` i_override_close
- `in_12` i_override_pip
- `in_13` i_override_allow_weak
- `in_14`..`in_23` layer toggles (i_show_*)

If we need to narrow back from the preset values during session, use `i_loose_preset=false`
then set the individual sliders.

## State-table verification

Both toggle states verified live during pre-flight:

| Preset | Disp gates row | Last DF row |
|---|---|---|
| OFF (canon) | `atr>=1.5, body>=0.6, cg<=0.25  [canon]` | `DF dn  2026-03-01` |
| ON (LOOSE) | `atr>=1.25, body>=0.55, cg<=0.35  [LOOSE]` | `DF dn  2026-05-13` |

The DF-row jump is meaningful: under loose gates, more recent DF events qualify (later DF
labels exist). Net effect: Pine is firing more MSS/DF events under loose, exactly as
intended.

## Screenshots captured

All under `~/mcp-servers/tradingview-mcp/screenshots/`:

| File | View | State |
|---|---|---|
| `preflight_canonical_jun2025_mar2026.png` | Full anchor window | canon |
| `preflight_loose_jun2025_mar2026.png` | Full anchor window | LOOSE |
| `preflight_canon_dec2025_mar2026.png` | Dec 2025 → Mar 2026 zoom | canon |
| `preflight_loose_dec2025_mar2026.png` | Dec 2025 → Mar 2026 zoom | LOOSE |

## Visual observations (per zoom)

### Full anchor window (Jun 2025 → Mar 2026)

Visible-MSS-label density rises noticeably under LOOSE. Specific anchor-level identification
is not possible at this zoom because labels overlap — use the Dec-Mar zoom for that.

### Dec 2025 → Mar 2026 zoom (DENSEST anchor region — 9 of 14 test anchors live here)

**Canonical state — MSS labels visible:** ~4 total
- 1 MSS up near Dec 24 / Jan 22 markers (upper-mid)
- ~2 MSS dn in the post-Mar 2 bear leg
- 1 MSS dn further right (May 2026)

**LOOSE state — MSS labels visible:** ~6–7 total. New fires that were absent in canon:
- **MSS up near "Dec 10 range birth?" marker** — likely the Dec 10 MSS_FIRE_TEST anchor firing (V005 expectation)
- Additional MSS up around mid-Jan area — possibly the Jan 20 MSS_FIRE_TEST anchor (V011 expectation: Jan 20 breaks Jan 12 SWH @ 1.16985)
- Slightly more density around Dec 22 area — possibly the Dec 22 MSS_FIRE_TEST anchor

**Pre-flight verdict:** Daily-loose preset IS adding MSS labels in the expected anchor
regions. Per-anchor confirmation (which specific bar gets the label, whether timing is
correct) requires Olya's live visual ruling — that is exactly what the session is for.

**Anchors that are NOT yet visually verified from these screenshots:**
- Jun 11, Jun 12, Oct 30 2025 (out of zoom range) — captured in the full-window screenshot but at unreadable density
- Mar 13 2026 (V015 combined anchor) — out of right edge of zoom

## What pre-flight does NOT establish

- Exact bar-by-bar correspondence between Pine MSS labels and the 22-anchor matrix — needs
  Olya screen-share + live navigation per anchor
- Whether the REGRESSION set (Feb 2026 swings + Mar 2 bear MSS) still holds under LOOSE —
  Feb anchors are out of the Dec-Mar zoom; need to verify during session via state table
  history or focused screenshots
- Whether the candidate `allow_weak_grade: true` is too loose (introduces spurious fires
  that Olya rejects) — this is what the narrow-back phase 2 of the session is for

## Procedure for live session (run order)

1. **Open `port/daily_displacement_params.yaml`** in editor; this is the verdict-capture
   target. Verification matrix has 22 rows, each with `pine_pre_flight` and `olya_verdict`
   columns.

2. **Confirm chart in canonical state** (state table → `[canon]`). Run `data_get_pine_tables`
   if uncertain.

3. **Walk Olya through the MSS_FIRE_TEST anchors first** (the load-bearing 7):
   - Toggle Daily-loose preset ON (`indicator_set_inputs in_6=true`)
   - For each of: 2025-06-11, 2025-06-12, 2025-10-30, 2025-12-10, 2025-12-22, 2026-01-20, 2026-03-13:
     - `chart_scroll_to_date` to the anchor
     - Olya rules PASS / FAIL / ADJUST
     - Capture verdict + Olya verbatim in YAML

4. **Then SURFACE_TEST anchors** (7 price-level confirmations) — these should also pass
   under LOOSE; if any regress, that's a signal.

5. **Then REGRESSION set** (8 anchors Olya already signed off on yesterday) — these are
   sanity checks; failure = LOOSE is too loose, narrow back.

6. **If regressions appear** (most likely): toggle preset OFF (`in_6=false`), set
   individual sliders to intermediate values. Iterate until all 22 anchors PASS.

7. **Capture deferred items** if Olya is available — see `open_data_capture_template.md`:
   - Jan 22 FVG range
   - Weekly low broken on Mar 13 — specific price
   - Mar 10 bearish range high — specific price
   - Jan 20 displacement bar params (Pine can compute these — see candidate values capture below)

8. **Lock the YAML** — once all 22 verdicts PASS: set `status: LOCKED`, `locked_date:
   2026-05-19`, write Olya verbatim to `locked_by` notes, commit.

## What this session does NOT do

Per stocktake §5 step_3 and en1gma CLAUDE.md §6:
- Does NOT write to `~en1gma/console/detection/locked_baseline.yaml` (V005 sprint scope)
- Does NOT promote any FVG/OB/BPR work (§6 standing stop)
- Does NOT touch M002 NEX Weekly Terrain (separate G-routing decision)
- Does NOT engage the renderer/persistence engineering lane (V002/V007/V008/V012/V016 — separate Level_2 sprint)

Output of THIS session: `~/pine_calibration/port/daily_displacement_params.yaml` LOCKED only.
Port to en1gma is V005 sprint, separately G-ratified per stocktake §4 d1.

## State at end of pre-flight

- Daily-loose preset: **OFF** (canonical defaults restored) ✅
- Chart visible range: Jun 2025 → Mar 2026 (anchor window)
- State table: `atr>=1.5, body>=0.6, cg<=0.25 [canon]` confirmed
- Indicator entity_id: `mVF5sg` (note for during-session toggle calls)

Ready for live session.
