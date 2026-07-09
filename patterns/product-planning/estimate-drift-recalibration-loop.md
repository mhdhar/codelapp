---
title: "Recalibrate the Roadmap as Actuals Land"
slug: "estimate-drift-recalibration-loop"
format: "loop"
category: "product-planning"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "advanced"
est_time: "5 min per ticket, ongoing"
models: ["any"]
summary: "Log estimate vs actual per finished ticket, compute a drift factor, and re-plan when the milestone math breaks."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're mid-build, tickets are landing, and the milestone date still rests on estimates made weeks ago that nobody has checked against reality. Start this after the first two or three tickets close, when there's enough history to calibrate but still enough runway to act on it.

## The pattern
```text
From now on, every time a ticket or task in this project is finished, run a calibration check.

Keep a ledger with one row per finished ticket:
- Ticket name and its original estimate. Pull estimates from the plan or tracker file in this repo, ask me once if they live somewhere you can't see.
- Actual effort: sessions or days it really took. Reconstruct this from the conversation history and git log timestamps, don't ask me.
- Drift ratio: actual divided by estimated.

After adding each row:
1. Recompute the running drift factor: the median ratio across all rows, ignoring the single worst outlier.
2. Multiply every remaining estimate by the factor and recompute the milestone landing date.
3. If the projected date slips past the target by more than 20%, stop and present three options: cut the lowest-value remaining ticket, move the date, or split the milestone. Don't pick one silently.
4. Otherwise report one line only: "Calibration: factor X.X, milestone still lands around DATE."

Past rows are frozen. Never restate an old estimate or actual to make the factor look better.
```

## Real example output
"Row 7: 'CSV import validation', estimated M (0.5 day), actual 1.5 days (git log: 14 commits across 2 sessions, July 21-22). Ratio 3.0.

Running factor: 1.6 (median of 7 rows, worst outlier row 3 ignored).
Remaining 5 tickets: 4.5 estimated days, calibrated to 7.2.
Projected landing: Aug 6 against an Aug 1 target. That's a 26% slip, over the 20% threshold.

Stopping. Options: (a) cut 'export templates' (P2, 1.5 calibrated days) and land Aug 1, (b) move the date to Aug 6, (c) split the milestone at 'billing flow' and ship the first half July 30. Your call."

## Why it works
Individual estimates are guesses, but the ratio between your guesses and reality is surprisingly stable within one project, so a median factor over a frozen history turns bad estimates into a usable forecast. The 20% tripwire is the teeth: slippage becomes an explicit three-option decision instead of a surprise the week the milestone was due.
