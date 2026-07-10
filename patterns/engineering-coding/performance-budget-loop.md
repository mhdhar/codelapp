---
title: "Performance Budget Loop"
slug: "performance-budget-loop"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Optimize against a stated numeric budget with median-of-5 measurements, reverting any change that doesn't move them."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["core-web-vitals-fix-loop", "one-variable-at-a-time", "task-to-self-verifying-brief"]
---

## When to use this
Something is measurably slow (an endpoint, a query, a build step) and you want it fixed with numbers, not a pile of "should be faster now" changes. Use it when the slowness is reproducible locally or on staging; guessing at production-only slowness needs profiling access first.

## The pattern
```text
Make the slow thing I point you at fast, against a measured budget, not by feel.

1. BASELINE: Build a repeatable measurement: a timed script, a benchmark, or a timed request against the local server. Run it 5 times and report the median, not the best run. Paste the numbers.
2. BUDGET: Propose a numeric target based on the baseline and what the operation is for (e.g. "p50 under 300ms for an interactive endpoint"). If I gave a target, use mine. No optimizing until a budget is stated.
3. PROFILE: Find where the time actually goes before changing anything. Name the top 1-2 costs with evidence: profiler output, EXPLAIN ANALYZE, timing logs. No changes on a hunch.

Loop:
4. Apply exactly one optimization aimed at the top measured cost.
5. Re-measure with the identical method, 5 runs, median. Paste before and after.
6. If the change didn't measurably help, revert it. "Probably faster" code that adds complexity is a net loss.
7. Repeat 4-6 until the budget is met, or the next change would buy under 10% while adding real complexity. In that case stop and report the remaining gap and what closing it would take.

Finish with a table: each change, median before, median after, kept or reverted.

Start by asking me what's slow and whether I have a numeric target, then wait. If I give no target, propose one in step 2.
```

## Real example output
```
> The slow thing: GET /api/search takes over 3 seconds against the staging dataset. No target in mind.

1. BASELINE: timed script, /api/search?q=invoice, 5 runs: 3,180 / 3,240 / 3,510 / 3,190 / 3,290ms. Median 3,240ms.
2. BUDGET: interactive search box, proposing p50 under 300ms. No target given, so using 300ms.
3. PROFILE: EXPLAIN ANALYZE shows 2,900ms of the total is one query: seq scan on documents (1.2M rows) because ILIKE '%term%' can't use the btree index.

Change 1: GIN trigram index on documents.title. Median 3,240ms to 410ms. Kept.
Change 2: enabled gzip on the response. 410ms to 405ms, inside run-to-run noise. Reverted.
Change 3: replaced exact COUNT(*) with an estimated count for the pagination header. 410ms to 185ms. Kept.

Budget met: median 185ms, target 300ms.

| Change            | Before  | After | Verdict  |
| Trigram index     | 3,240ms | 410ms | kept     |
| Response gzip     | 410ms   | 405ms | reverted |
| Estimated count   | 410ms   | 185ms | kept     |
```

## Why it works
Without a stated budget, optimization has no finish line, and every plausible tweak gets kept. Median-of-5 with an identical method stops the agent from chasing measurement noise, and the mandatory revert rule means the final diff contains only changes that provably paid for themselves.
