---
title: "Call Your A/B Test: Ship, Kill, or Keep Running"
slug: "ab-test-ship-kill-verdict"
format: "goal"
category: "marketing-growth"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Feed in variant numbers, get a verdict with the math shown and a date if the answer is keep running."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["pre-registered-demand-test-design", "headline-gauntlet-loop", "weekly-growth-lever-review"]
---

## When to use this
An A/B test has been running for a while, the dashboard shows the variant "up 8%", and someone wants to ship it today. Before that happens, you want an honest read: is this a real effect, noise, or a broken test? Not for tests you haven't run yet.

## The pattern
```text
Give me a verdict on my A/B test. Exactly one of: SHIP, KILL, KEEP RUNNING, or INVALID TEST. Then make the case.

Your task:
1. Compute conversion rate for control and variant, absolute and relative lift, and a 95% confidence interval on the difference. Show the arithmetic. Do not just assert "significant".
2. Sanity-check before trusting the numbers:
   - Sample ratio mismatch: compare the actual traffic split to the intended split. If it deviates beyond what chance explains, the test is INVALID; do not make a product decision from the data.
   - Runtime: if the test has run less than two full weekly cycles, flag that weekday and weekend visitors may differ.
   - Peeking risk: if I've been checking daily and this is one of many looks, say the nominal confidence is overstated.
3. Verdict rules:
   - INVALID TEST: a sample-ratio mismatch or other integrity failure means the data cannot support a product decision. State the repair or rerun needed.
   - SHIP: the interval excludes zero in the variant's favor and every applicable sanity check passed.
   - KILL: the interval excludes zero against the variant, or the best plausible upside (the top of the interval) is too small to justify keeping the code.
   - KEEP RUNNING: the interval spans zero. Then answer the question that actually matters: given the observed effect size and current daily traffic, how many more days until the test can be decisive, and is that worth waiting for? If the needed sample is months away, say so and recommend KILL for futility.
4. End with the verdict word and the single number that drove it.

Do not hedge with "it depends." One verdict, stated first.

Start by asking me in one message for the test data: intended split, days running so far, and visitors and conversions for each variant. Wait for my reply before computing anything.
```

## Real example output
KEEP RUNNING, but only 9 more days.

Control: 14,200 visitors, 412 conversions, 2.90%. Variant: 14,050 visitors, 445 conversions, 3.17%. Relative lift 9.2%. 95% CI on the difference: -0.13pp to +0.66pp. The interval spans zero, so the 9.2% lift is not yet distinguishable from noise.

Sanity checks: split is 50.3/49.7 against an intended 50/50, within chance. 11 days running covers one full weekly cycle plus a partial, acceptable.

At the observed effect size you need roughly 21,500 visitors per arm for a decisive read. At current traffic (about 1,300/arm/day) that's 6 more days of data, call it 9 with a buffer. Decision date: check again on the 18th. If the interval still spans zero then, KILL for futility rather than letting it run forever.

Driving number: the CI floor of -0.13pp. The variant could still be losing.

## Why it works
Forcing one of three words prevents the analyst non-answer that pushes the decision back to whoever asked. The sample ratio mismatch check catches the surprisingly common case where the test infrastructure, not the copy, produced the difference. And making KEEP RUNNING come with a date and a futility rule turns it from a stall into a decision.
