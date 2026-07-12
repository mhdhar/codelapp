---
title: "One Variable at a Time"
slug: "one-variable-at-a-time"
format: "prompt"
category: "debugging-evaluation"
tools: ["universal"]
difficulty: "beginner"
est_time: "2 min"
models: ["any"]
summary: "A standing rule: one change per run while debugging, predict before you edit, revert what didn't help."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["root-cause-debugging-loop", "performance-budget-loop", "dependency-upgrade-breakage-isolation"]
---

## When to use this
Drop this in at the start of a debugging session, before the agent starts editing. It prevents the classic spiral: three speculative changes go in between runs, the bug disappears, and now nobody knows which change fixed it or what the other two broke.

## The pattern
```text
Standing rule for this debugging session: one variable at a time.

- Change one testable hypothesis at a time. An atomic change set may include
  production code, its regression test, and necessary instrumentation, but it
  must not test two candidate explanations in the same run.
- Before each change set, write one line: the hypothesis, what you're changing,
  and what you predict the output will do if the theory is right.
- After the run, compare against the prediction. If the failure didn't improve,
  revert changes disproven by the run before trying the next idea. Keep a valid
  regression test or diagnostic only when the ledger records what evidence it adds.
- If two changes testing separate hypotheses ever go in together and the bug goes
  away, split and re-test them; do not split an atomic change set that tests one hypothesis.
- Keep a running ledger in each message: attempt number, change,
  prediction, result, kept or reverted.
- End state: the working tree contains only changes proven necessary for the
  behavior or valid test evidence, and the ledger shows why each one is there.

This rule holds for the rest of the session, not just the next message.
```

## Real example output
Ledger after finding a login redirect bug:

| # | Change | Prediction | Result | Status |
|---|---|---|---|---|
| 1 | Bump session TTL 15m to 60m in `auth.config.ts` | Redirect loop stops | Loop persists, identical | Reverted |
| 2 | Await `refreshToken()` in `middleware.ts:27` | Loop stops | Loop stops, but logout breaks | Reverted |
| 3 | Move `refreshToken()` before the redirect check, still awaited | Loop stops, logout intact | Both pass, 10/10 runs | Kept |

Final diff: 2 lines in `middleware.ts`. Attempts 1 and 2 left nothing behind, confirmed with a clean working tree check before attempt 3.

## Why it works
Each run is an experiment, and an experiment with two changed variables produces no causal information: you learn nothing you can act on, whatever the outcome. Predictions written before the run stop after-the-fact rationalizing, and the revert discipline means the final diff is the explanation, with no speculative edits riding along into review.
