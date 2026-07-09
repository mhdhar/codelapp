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
---

## When to use this
It's the start of the week, you have last week's basic funnel numbers (visits, signups, activation, whatever your key action is), and you want one focused thing to work on instead of a scattered list of "growth ideas."

## The pattern
```text
Step 1: Below the line at the end of this block, paste your funnel numbers for the last 2-4 weeks, one row per week:
Week | Visits | Signups | your key action (activated, paid, retained, whatever it is; name it in the header)

Step 2: Diagnose, don't brainstorm:
Look at this funnel data across the weeks given. Do not suggest generic growth tactics.
1. Calculate the conversion rate between each stage for every week.
2. Identify which single stage has the worst conversion rate relative to the others, or the sharpest week-over-week drop.
3. State that stage as the one highest-leverage lever for next week. Just one, not a list.
4. Show your reasoning: the actual numbers that led you there.
5. Propose exactly one concrete experiment to run against that stage next week, specific enough to start today.
6. Name the one metric that will tell us by next week's review if it worked.

Step 3: If the agent hedges or gives multiple levers, tell it: "Pick one. Which single stage, if fixed, moves the topline number the most?" and ask it to answer again.

Paste your weekly funnel rows below this line:
```

## Real example output
Visits -> Signup conversion: 3.1%, 3.4%, 3.0%, 3.3% (stable).
Signup -> Activated conversion: 61%, 58%, 41%, 39% (dropped sharply in weeks 3-4).

Highest-leverage lever: activation, not top-of-funnel. Visits and signup rate are stable, so more traffic won't fix anything right now. Activation dropped 20 points in two weeks while signup volume held steady, meaning something in onboarding broke or got harder, not that fewer good users are arriving.

Experiment: audit what changed in onboarding between week 2 and week 3 (new steps, copy changes, a broken redirect). Ship a fix and re-measure.

Metric to watch: signup-to-activation rate, target back above 55% by next review.

## Why it works
Forcing the agent to show stage-by-stage math before naming a lever stops it from defaulting to "run more ads" or other generic advice. Refusing to accept a list and demanding one answer keeps the output actionable instead of a wish list.
