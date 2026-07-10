---
title: "Find the Retention Cohort That's Actually Bleeding"
slug: "retention-cohort-lever-review"
format: "workflow"
category: "marketing-growth"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Feed an agent cohort retention data and get one diagnosed churn point, not generic save-the-user tactics."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["onboarding-sequence-from-activation-steps", "weekly-growth-lever-review", "voice-of-customer-review-synthesis"]
---

## When to use this
You have month-over-month or week-over-week cohort retention data and retention feels "bad," but you don't know if it's a slow universal decay (normal) or a specific cohort that broke (fixable). You want a diagnosis before you brainstorm win-back campaigns.

## The pattern
```text
Step 1: Ask me to paste my cohort retention table, one row per signup cohort, one column per period since signup:
Cohort | Period 0 | Period 1 | Period 2 | Period 3 | ...
Wait for my reply. Don't diagnose until you have it.

Step 2: Diagnose, don't brainstorm:
1. Calculate the period-over-period drop-off rate for every cohort.
2. Compare cohorts against each other, not just against time. Determine: is every cohort decaying at roughly the same shape (systemic, expected), or do recent cohorts drop off faster at the same point in their lifecycle than older cohorts did (something specific broke)?
3. Identify the single period (e.g. "period 0 to period 1") and cohort range where the steepest, most anomalous drop is.
4. State that as the one lever, not a list. If the data shows a systemic decay with no anomaly, say so explicitly instead of forcing a fake lever.
5. Show the numbers that led you there.
6. Propose exactly one experiment specific enough to start today, tied to what plausibly changed around when the anomaly started (a shipped change, a pricing change, an onboarding edit).
7. Name the one metric and target that tells us by next review if it worked.

Step 3: Before you deliver the answer, check it. If it hedges or lists multiple levers, redo it against one question: which single period, if fixed, saves the most users? Pick one.
```

## Real example output
Cohort Jan: P0 100%, P1 62%, P2 51%, P3 47%
Cohort Feb: P0 100%, P1 58%, P2 44%, P3 -
Cohort Mar: P0 100%, P1 41%, P2 -, P3 -

Diagnosis: this is not a systemic decay curve. Jan's P0-to-P1 drop was 38 points. Feb's was 42 points. Mar's is 59 points, nearly 20 points worse than Jan at the exact same lifecycle stage, and the trend is getting worse cohort over cohort, not stable. This points to something specific breaking in the first-period experience starting around early February, not a general "people churn eventually" pattern.

Lever: Period 0-to-1 retention, Feb and Mar cohorts specifically.

Experiment: pull the changelog for the two weeks before the Feb cohort signed up. Check whether the day-3 upgrade prompt that shipped Jan 28 is showing to free users before they've had a first successful session, and if so, gate it behind first-value instead.

Metric to watch: Period 0-to-1 retention for the next full cohort, target back above 55%.

## Why it works
Comparing cohorts to each other at the same lifecycle stage, not just charting retention over time, is what separates "this is normal decay" from "something we shipped broke onboarding." Refusing to accept a scattershot list of tactics keeps the output tied to one fixable cause instead of five vague re-engagement ideas.
