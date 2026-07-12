---
title: "Find Next Week's Highest-Leverage Growth Move"
slug: "weekly-growth-lever-review"
format: "workflow"
category: "marketing-growth"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Feed an agent your weekly metrics and get one ranked growth lever with the reasoning shown, not five vague ideas."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["retention-cohort-lever-review", "channel-budget-allocation-decision", "ab-test-ship-kill-verdict"]
---

## When to use this
It's the start of the week, you have last week's basic funnel numbers (visits, signups, activation, whatever your key action is), and you want one focused thing to work on instead of a scattered list of "growth ideas."

## The pattern
```text
Step 1: Ask me to paste my funnel numbers for the last 2-4 weeks, one row per week:
Week | Visits | Signups | my key action (activated, paid, retained, whatever it is; I'll name it in the header)
Wait for my reply. Don't diagnose until you have the rows.

Step 2: Diagnose, don't brainstorm:
Look at this funnel data across the weeks given. Do not suggest generic growth tactics.
1. Calculate the conversion rate and denominator between each stage for every week.
2. Flag any stage where the counts are too small or noisy for a reliable comparison.
3. Identify the single bottleneck hypothesis with the worst conversion rate or sharpest week-over-week drop. Funnel data alone does not prove the cause.
4. Show your reasoning: the actual numbers and uncertainty that led you there.
5. Propose exactly one concrete experiment to test that hypothesis next week, specific enough to start today.
6. Name the one metric that will tell us by next week's review whether the experiment improved that stage.

Step 3: Before you deliver the answer, check it. If it hedges or gives multiple hypotheses, redo it against one question: which single stage is the best evidence-backed experiment to test next? If the data is insufficient, say that instead of forcing a lever.
```

## Real example output
Visits -> Signup conversion: 3.1%, 3.4%, 3.0%, 3.3% (stable).
Signup -> Activated conversion: 61%, 58%, 41%, 39% (dropped sharply in weeks 3-4).

Best bottleneck hypothesis to test: activation, not top-of-funnel. Visits and signup rate are stable while activation dropped 20 points in two weeks. That makes activation the clearest anomaly to investigate, not proof that onboarding caused it.

Experiment: audit what changed in onboarding between week 2 and week 3 (new steps, copy changes, a broken redirect). Ship a fix and re-measure.

Metric to watch: signup-to-activation rate and its weekly denominator, with a target back above 55% only if the baseline sample is large enough to make that comparison meaningful.

## Why it works
Forcing the agent to show stage-by-stage math before naming a lever stops it from defaulting to "run more ads" or other generic advice. Refusing to accept a list and demanding one answer keeps the output actionable instead of a wish list.
