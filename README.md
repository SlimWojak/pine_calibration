# pine_calibration

Sandbox lab for calibrating the **HTF MAP** algorithm visually in TradingView Pine Script v6, before porting the locked-in logic back to `~en1gma`'s Python codebase.

## Why this repo exists

Tuning the HTF MAP through verbose human-review docs has been slow and confusing. This repo is a deliberate detour: build the algorithm in Pine, project it onto live charts, judge fires visually, iterate fast, lock the logic, then port back to Python with a signal-diff check to confirm parity.

**This repo is not the source of truth.** `~en1gma` is. This is a calibration workbench.

## Stack

- **TradingView Desktop** (Premium) with Chrome DevTools Protocol enabled on port 9222
- **Claude Code** with two MCP servers wired globally:
  - `tradingview` — inject/compile Pine, read charts, bar replay, screenshots
  - `pinescript-docs` — Pine v6 reference + code review (no v5 hallucinations)
- **Pine Script v6**

## Workflow

1. Write Pine v6 in `pine/`
2. Inject + compile via MCP, fix errors
3. Apply to chart in TradingView
4. Step through bars via replay mode, watch fires
5. Judge visually, tune, repeat
6. Once locked, port to Python in `~en1gma`
7. Signal-diff: run Pine and Python side-by-side on the same data, confirm parity

## Layout (will grow as needed)

```
pine/        # Pine v6 source
notes/       # Brief, decisions, observations during calibration
diffs/       # Signal-diff results when porting back to Python
```

## Related

- `~en1gma` — main system, target for the ported algorithm
- `~research_accelerator` — the precedent that made this approach obvious (visual LTF tuning)
