# Olya session walkthrough — Mar 2 bearish flip calibration

**Goal of the session:** Lock the direction-flip rule (A / B / C) for the HTF MAP V1, using the Mar 2 fixture as the adjudicating case. If time permits, also surface anything she flags on the swing/displacement primitives on this window.

**Session shape:** screen-share TradingView Desktop with G + CTO + Olya. ~30-45 min. CTO drives the chart, G frames the question, Olya rules.

---

## Setup checklist (do this in the 10 min before Olya joins)

1. TradingView Desktop open at `https://www.tradingview.com/chart/PAGInqdu/`, FX:EURUSD, 1D
2. Indicator `HTF MAP V1 — calibration sandbox` loaded on chart (default mode: **C_HYBRID**)
3. Visible range set to roughly Dec 1 2025 → May 17 2026 (so all 5 pivotal dates fit)
4. Pine Editor panel CLOSED (full chart real estate for Olya)
5. State-readout table visible top-right
6. All 5 pivotal date labels visible on the chart (Dec 10, Dec 24, Jan 22, Mar 2, Mar 3)
7. `scorecard.md` and `fixtures/2026-03-02_bearish_flip.md` open in a second window — for note-taking + verbatim quote capture
8. Replay quickly tested: `replay_start(date=2026-02-25)`, step a few times, `replay_stop`. Confirm clean.

If anything in the checklist fails, fix BEFORE Olya joins.

---

## Question sequence (in this order — don't deviate)

### Q1 — Mode adjudication (the load-bearing question, ~5 min)

Show Olya the chart in **Mode C_HYBRID** first. Then:
- Open indicator settings → switch to **A_DELIVERY_FAILURE** → screenshot side: stuck pink, only DF labels
- Then switch to **B_REPLACEMENT** → screenshot side: clean tri-color, no DF labels
- Then back to **C_HYBRID** → tri-color + DF labels

Ask: **"When an opposite-direction Daily MSS fires before the prior range's First Draw is reached, which of these three should the map do — and is that the rule for ALL such events or only some?"**

Capture her exact words. If she picks C, ask: "Always? Or only when X?" — get the boundary condition.

### Q2 — Mar 2 date alignment (~10 min)

With chart in her chosen mode, scroll/zoom to **Feb 24 → Mar 6**.
Run bar replay from `2026-02-25` and step bar-by-bar through to Mar 6.

At each bar, the state-readout table updates: she sees direction, last MSS, last DF live.

Ask: **"Does the bearish MSS belong on 2 Mar specifically, or wherever the Pine first finds a valid SL break? If the Pine's flip date doesn't match what you see — is the issue with my swing definition, my displacement gate, or your mental anchor?"**

Possibilities to be ready for:
- "The Pine missed a sub-SL that the 15-pip filter excluded" → action: relax filter for Daily? (Olya's call)
- "Mar 2 is a regime-shift shorthand; the actual mechanical break can be any day after" → action: lock the Pine behavior as-is
- "Your displacement gate is too strict on Mar 2 — that bar IS the trigger" → action: examine close_loc / body_ratio on the Mar 2 bar

### Q3 — Sub-question coverage (~10 min)

Briefly walk through the other pivotal dates (no deep dive — just flag whether Pine matches her view):
- **Dec 10**: pivotal blue label visible. Does the chart show range birth here? Should the Pine surface this as a discrete event?
- **Dec 24**: was First Draw hit? Should daily_state go to REASSESSMENT here?
- **Jan 22**: range succession — does she see one Expansion or multiple range births? How should Pine render this?

For each, get a 1-sentence verdict. Capture in `scorecard.md`.

### Q4 — Primitives spot-check (~5 min, if time)

Ask Olya: **"Look at the swing crosses and yellow/aqua displacement markers. Anything you'd expect to see that's missing, or anything firing that shouldn't?"**

This is the parity check between Pine and her mental model of the locked primitives. Surfaces calibration drift if any.

---

## What gets captured at end of session

1. **`port/direction_flip_rule.yaml`** — write the locked rule contract using the `_template.yaml` shape. Include Olya's exact words in `selection_evidence`.
2. **`scorecard.md`** — flip `2026-03-02_bearish_flip` row to **PASS** (or **NEEDS REWORK** + describe what)
3. **`fixtures/2026-03-02_bearish_flip.md`** — append `## Olya verdict (2026-MM-DD)` section with her ruling verbatim
4. **`screenshots/`** — at minimum: the final Pine state with her chosen mode active and the chart at the Mar 2 area
5. **For the other fixtures she touched**, populate the stubs with her quick verdicts so we can iterate them next session

If `direction_flip_rule.yaml` is locked, that's the artifact G ratifies and we port to Python in `~en1gma`. Until then, no Python port-back.

---

## What NOT to do during the session

- Don't ask her to read MD docs. She's here for the chart.
- Don't ask "how should we implement X" — she's the methodology authority, not the engineer.
- Don't make code changes live in front of her. Capture rulings, implement after.
- Don't agree to scope expansion in real-time ("can the Pine also show…?") — note them, route through G post-session.
- Don't lose the Mode A/B/C screenshots. They're the calibration evidence.
