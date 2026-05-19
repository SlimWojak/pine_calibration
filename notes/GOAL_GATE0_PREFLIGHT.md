# GOAL Gate 0 Preflight - 2026-05-19

status: FAILED - HALT_11
operator: Codex GOAL CTO/orchestrator
window: EUR/USD Daily 2024-01-01..2026-03-31
scope: PHASE_A_ONLY (swing_points, displacement, mss)

## Anchor Tuple

| Repo | Path | HEAD | Branch | Status |
|---|---|---|---|---|
| pine_calibration | `/Users/echopeso/pine_calibration` | `d7536bb3792de50630d9f6740920a25642e273c7` | `main` | clean at Gate 0 start |
| en1gma | `/Users/echopeso/en1gma` | `849608e090553f1ab016af8c233f202308ef7846` | `main` | one pre-existing untracked file: `docs/raw/OLYA_TRADE_011_ADDENDUM_PRE_GATE_1.md`; no writes made |
| research_accelerator | `/Users/echopeso/research_accelerator` | `b7055f2b5a5d40239366cf3737431a9b93b428b6` | `main` | clean at Gate 0 start |

## Gate 0 Checks

| Check | Result | Evidence |
|---|---|---|
| GATE0_1 fixed Daily window | PASS | `2024-01-01..2026-03-31`, symbol intent EUR/USD Daily |
| GATE0_2 River coverage smoke | PARTIAL / NOT PASS | Brief command `python3 site/detect.py --start 2024-01-01 --end 2026-03-31` failed because this checkout requires `--config` and `--output`. Resolved command: `python3 site/detect.py --start 2024-01-01 --end 2026-03-31 --config configs/locked_baseline.yaml --output /tmp/pine_gate0_ra_smoke_20260519`. It generated non-empty output through 32 of 118 weeks before the run was stopped after HALT_11 became decisive. Output evidence: 96 files under `/tmp/pine_gate0_ra_smoke_20260519`, including detections/candles/sessions for `2024-W01..2024-W32`. |
| GATE0_3 TradingView cloud-save matches local Pine | FAIL | TradingView saved script `HTF MAP V1 - calibration sandbox`, `pineVersion=21.0`, SHA256 `c89e9fa02573862703b85a3b3c3c837117f3c549cc71312d6828651c59d461b5`, 393 lines. Local `pine/htf_map_v1.pine` SHA256 `52144572871c47dbc109a3577edf0e8929321e7d9213166b339b966dfb056407`, 564 lines. Exact match: false. |
| GATE0_4 runner | PASS | Runner is Codex GOAL. |
| GATE0_5 displacement YAML lock | FAIL / UNRESOLVED | `port/daily_displacement_params.yaml` currently has `status: DRAFT`; Gate 0 requires LOCKED at canon HTF values per CODE WINS plus 2026-05-19 noise-sweep evidence. No YAML lock edit was made after HALT_11. |
| GATE0_6 Phase split | PASS | Phase A only; Phase B deferred. |
| GATE0_7 V005 hold and en1gma HEAD freeze | PASS AT START | V005 held pending Phase A completion per brief; start HEAD recorded above. End freeze not reached because Gate 0 halted. |
| GATE0_8 research_accelerator and pine_calibration HEAD | PASS AT START | Start HEADs recorded above. |

## TradingView Cloud Mismatch Details

TradingView app was reachable through CDP on `FOREXCOM:EURUSD`, resolution `1D`.

Loaded study:

```text
entity_id: mVF5sg
name: HTF MAP V1 - calibration sandbox
pineId: USER;0bde1455c32e4802bccc55eb8432f9c7
pineVersion: 21.0
```

Current chart inputs showed `in_6=true`, so the chart table reported Daily-loose gates:

```text
Disp gates | atr>=1.25, body>=0.55, cg<=0.35 [LOOSE]
```

The saved cloud source also differs behaviorally from local:

- cloud source still includes `i_override_allow_weak`
- cloud source still derives `eff_allow_weak_override`
- cloud source still allows override on WEAK grade
- cloud source still uses the old cluster/single precedence shape
- local source has fixes #7-11 and removes the weak override knob

Because GATE0_3 requires the cloud source SHA to match local before edits, Phase A primitive work did not start.

## Decision

HALT_11 is triggered: TradingView cloud-save state cannot be verified as matching local Pine before edits.

No Pine edits, no en1gma writes, no research_accelerator writes, no Phase B work, no V005 preempt.
