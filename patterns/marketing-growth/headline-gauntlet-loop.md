---
title: "Run Your Headline Through the Gauntlet Until It Survives"
slug: "headline-gauntlet-loop"
format: "loop"
category: "marketing-growth"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Score your landing page headline against a 5-check rubric, rewrite, rescore, and loop until it passes all 5."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["headline-subject-line-variants", "ab-test-ship-kill-verdict", "rubric-grading-loop"]
---

## When to use this
You have a landing page headline that feels fine but converts like it's invisible, and asking an agent to "make it punchier" just swaps one vague line for another. This makes the rewrite loop run against fixed criteria instead of taste.

## The pattern
```text
Pressure-test my landing page headline in a loop against this rubric. Five checks, pass or fail each:

1. Specific: names who it's for or what it does. A stranger could not paste it onto a competitor's site unchanged.
2. Outcome: states what the visitor gets, not what the product is.
3. Plain: no category jargon. "Platform", "solution", "seamless", "supercharge", and "AI-powered" as filler all fail this check.
4. Falsifiable: makes a claim someone could disagree with. "Work smarter" fails. "Close your books in 3 days, not 3 weeks" passes.
5. Skimmable: 12 words or fewer, reads in one breath.

Each round:
- Score the current headline on all 5 checks. Quote the exact words that make a check fail.
- Rewrite to fix ONLY the failing checks. Keep the words that passed.
- Rescore the rewrite in a table: check | pass or fail | why, quoting the words.

Loop without asking me anything until one headline passes all 5. Cap it at 6 rounds. If nothing passes by round 6, show the best scorer and name the one check it cannot pass without changing the actual offer, not the wording. When done, show the winner plus the two strongest runners-up from earlier rounds.

Only use claims and numbers I give you. If you need a number to pass check 4 and I gave none, use a falsifiable mechanism claim instead of inventing a stat.

First ask me in one message for my current headline, one line on what the product does, and any real numbers I can claim. Wait for my reply, then start round 1.
```

## Real example output
Round 1: "The all-in-one platform for modern finance teams." Fails 4 of 5 ("all-in-one platform" fails Plain; no outcome; nothing falsifiable; could be any fintech's headline).

Round 3 winner: "Close the month in 3 days. Your auditors see every number's source." Passes all 5.

Runners-up: "Month-end close in 3 days, with an audit trail attached." / "Stop closing the books in spreadsheets. 3-day close, every number traceable."

## Why it works
"Make it better" gives the model no stopping condition, so it wanders. A pass/fail rubric turns headline writing into a verification loop: every rewrite is checked against the same 5 gates, failures are quoted not vibed, and the loop has a hard exit. The no-invented-numbers rule matters because check 4 tempts the model to fabricate a stat to pass.
