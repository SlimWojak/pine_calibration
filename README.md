# pine_calibration

Sandbox lab for calibrating the **HTF MAP** algorithm visually in TradingView Pine Script v6, before porting the locked-in logic back to `~en1gma`'s Python codebase.

**Live state:** see [`scorecard.md`](scorecard.md) for pass/fail status across the 5 Gate 2 fixtures. The fixture under active calibration is named there.

## Why this repo exists

Tuning the HTF MAP through verbose human-review docs has been slow and confusing. This repo is a deliberate detour: build the algorithm in Pine, project it onto live charts, judge fires visually with Olya screen-sharing, lock the logic, then port back to Python with a signal-diff check to confirm parity.

**This repo is not the source of truth.** `~en1gma` is. This is a calibration workbench.

## Stack

- **TradingView Desktop** (Premium) with Chrome DevTools Protocol enabled on port 9222
- **Claude Code** with two MCP servers wired globally:
  - `tradingview` — inject/compile Pine, read charts, bar replay, screenshots, UI control
  - `pinescript-docs` — Pine v6 reference + code review (no v5 hallucinations)
- **Pine Script v6**

## Workflow per fixture

1. Read the fixture card in `fixtures/<date>_<slug>.md` — what should fire, per Olya
2. Look up any unfamiliar Pine functions via `pinescript-docs` MCP
3. Write/edit Pine in `pine/htf_map_v1.pine` — mirror locked params from en1gma verbatim
4. Inject + compile via `tradingview` MCP, fix errors, add to chart
5. Screen-share with Olya, walk her through the chart, capture her ruling
6. Capture the locked rule in `port/<rule>.yaml` using `_template.yaml` shape
7. Update `scorecard.md` row to PASS
8. Port to Python in `~en1gma`, signal-diff for parity (the contract is byte-identical event timestamps)

## Layout

```
pine/         Pine v6 source (htf_map_v1.pine is the single indicator)
fixtures/     Gate 2 calibration cards (one per pivotal date) + README
notes/        Iteration journal + Olya session walkthrough — terse, what fired, what's odd
port/         Locked rule-contract YAMLs (handoff to Python; _template.yaml = shape)
screenshots/  Visual evidence per iter
scorecard.md  Living pass/fail status across the 5 fixtures
CLAUDE.md     Hard rules + learnings for AI/CTO sessions
```

## Related

- `~en1gma` — main system, source of truth for the HTF MAP spec and target for the ported algorithm
- `~research_accelerator` — the precedent that made this approach obvious (visual LTF tuning)
