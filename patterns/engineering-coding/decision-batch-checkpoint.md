---
title: "Decision Batch Checkpoint"
slug: "decision-batch-checkpoint"
format: "loop"
category: "engineering-coding"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Collect small pending decisions during a long task and approve them together at one checkpoint."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["decision-log-append", "ambiguity-halt-and-ask", "ui-approval-gate-checkpoints"]
---

## When to use this
A task is long enough that it'll naturally hit several small forks in the road, naming, minor UX choices, config defaults, and you don't want to be pulled out of what you're doing for each one individually. Use this on any multi-step task where you'd rather batch-approve than get pinged repeatedly.

## The pattern
```text
Work through the task I give you. You'll likely hit several small decisions along the way that don't block progress but that I'd want a say in. Don't interrupt me for each one individually.

Loop:
1. As you work, when you hit a small decision (naming, a minor default, a UI copy choice, an ordering choice) that isn't blocking - pick a reasonable default, keep working, and add it to a running PENDING DECISIONS list instead of stopping to ask.
2. Only stop immediately for something genuinely blocking, this loop is for the non-blocking ones.
3. Continue until the task is fully done, or until you've accumulated 5+ pending decisions, whichever comes first.
4. At that checkpoint, present the full PENDING DECISIONS list at once, each with the default you picked and a one-line reason, and ask for a single batch approval or corrections.
5. Apply any corrections, then continue.

First ask me for the task, then wait. Don't start until you have it.
```

## Real example output
```
> The task: build a CSV export feature for the orders table.

Worked through it. 4 pending decisions accumulated, checkpoint:

PENDING DECISIONS:
1. Filename format - picked orders-export-YYYY-MM-DD.csv. Reason: matches the date format already used in the invoices export.
2. Column order - picked id, customer, total, status, date. Reason: matches the on-screen table's current column order.
3. Empty filter result - picked "export a CSV with headers only, no rows" rather than blocking the export button. Reason: consistent with how the table itself just shows "no results" without disabling anything.
4. Currency formatting in the export - picked raw numbers (42.50) not formatted strings ($42.50). Reason: CSVs are usually re-imported into spreadsheets, and pre-formatted currency strings break re-import math.

All 4 are working as described. Approve as-is, or correct any before I mark this done?
> Change #4 - actually keep the $ formatting, ops team pastes these straight into a report, doesn't re-import them.

Applied: export now uses formatted currency ($42.50). Task done.
```

## Why it works
Asking about a filename convention the moment it comes up breaks flow for a decision that almost never matters enough to justify an interruption. Batching the non-blocking ones into a single checkpoint gets you the same control with a fraction of the interruptions, while true blockers still stop the agent immediately.
