---
title: "Performance Regression Profiling Loop"
slug: "performance-regression-profiling-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Force a profiler measurement before and after each fix attempt so you're not guessing at what's slow."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Something got slower and the AI's instinct is to rewrite the first loop or query it sees ("this looks inefficient") without measuring anything. You want every claim of "this is the bottleneck" and every claim of "this fixed it" backed by a real number.

## The pattern
```text
We have a performance regression.

Follow this loop, do not skip the measurement steps:

1. Establish a real baseline. Pick a measurement command that fits this
   repo's stack (an existing benchmark script, a load tool like
   autocannon for HTTP, hyperfine for CLIs, EXPLAIN ANALYZE for a
   query), show me the command, then run it against the current slow
   code and paste the actual numbers (timings, profiler output, or query
   plan). Do not estimate, run it.
2. From that output, identify the single largest contributor to the
   total time (the hottest function, the slowest query, the biggest
   allocation). Name it and quote the number next to it. Do not pick a
   contributor based on what "looks inefficient" if the measurement
   doesn't back it.
3. Form one hypothesis for why that specific thing is slow, based on the
   code at that location, not general performance folklore.
4. Make the smallest possible change to test that hypothesis.
5. Re-run the exact same profiling/timing command from step 1. Paste the
   new numbers next to the old ones.
6. If the number didn't meaningfully improve, revert the change and go
   back to step 2 with the next-largest contributor from the original
   profile. Do not stack unverified changes on top of each other.
7. Repeat until the measured number is back at or below the target, then
   report the full before/after comparison.

Before step 1, ask me in one message what got slower and what the
target is (e.g. "GET /orders, ~600ms p50, was ~80ms"), then wait. If I
don't know the target, use "back to where it was before the regression."
```

## Real example output
Step 1: Ran `autocannon -c 10 -d 20 http://localhost:3000/orders` before any change: p50 612ms, p99 1180ms.

Step 2: Attached a Node `--prof` profile. Top contributor: `formatOrderLine` at 71% of self time, called once per order line inside a nested loop in `src/services/orders.ts:88`.

Step 3: Hypothesis: `formatOrderLine` re-fetches the currency formatter (`new Intl.NumberFormat(...)`) on every call instead of reusing one instance, and it's called ~40 times per request.

Step 4: Hoisted the `Intl.NumberFormat` instance out of the loop, created once per request instead of per line.

Step 5: Re-ran the same `autocannon` command: p50 94ms, p99 210ms.

Step 6: Improvement is well past the 80ms baseline target already, stopped here instead of chasing the next contributor.

Final: p50 612ms → 94ms, p99 1180ms → 210ms, root cause was `Intl.NumberFormat` construction inside a per-line loop, not the database query the report originally suspected.

## Why it works
Requiring a real profiler number before naming a suspect stops the model from optimizing the code that "looks slow" while the actual hot path goes untouched. Re-measuring after every single change, and reverting anything that doesn't move the number, prevents a pile of unverified "probably faster" edits that make the code harder to read without proving any of them helped.
