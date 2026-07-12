---
title: "Adversarial Fix Verification Loop"
slug: "adversarial-fix-verification-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "15 min"
models: ["any"]
summary: "Have a second, fresh-context pass try to prove the first agent's fix is wrong before you trust it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["verify-before-claiming-fixed", "hostile-reviewer-pass", "no-fix-without-a-failing-test"]
---

## When to use this
An AI agent (possibly in an earlier session or a different tool) says a bug is fixed and you're about to merge it. You want a genuinely independent pass that tries to break the fix, not a rubber stamp from the same context that wrote it.

## The pattern
```text
You are reviewing a fix made by a different session. You did not write
this fix and have no stake in it being correct. Your job is to try to
prove it is wrong or incomplete, not to confirm it.

If the bug's expected behavior and reproduction are not already in the
task or available context, ask me for those missing details in one
message and wait. Otherwise, find the fix yourself: check the
uncommitted working tree diff first (`git status`, `git diff`), then
recent commits (`git log`) for the change that claims to fix this bug.
Tell me which diff you're reviewing before you start.

If you can see the original agent's reasoning, state `NOT INDEPENDENT`.
For a genuinely independent review, use a separate session that has the
bug contract and diff but not the original fix rationale.

Do this in order:

1. Read the actual changed code yourself. Do not trust any summary of
   what the fix does, verify it against the real diff.
2. State the narrowest possible interpretation of what this fix actually
   guarantees, not what it was intended to fix.
3. List the 3 highest-value distinct inputs, edge cases, or call sites
   NOT covered by the original bug report that could still hit the same
   underlying issue. If fewer than 3 material risks exist, list all of
   them and explain why; do not pad the review with hypothetical risks.
   Be concrete (e.g. "empty array instead of null", "a second call site
   that reaches this function", "concurrent requests instead of one").
4. For each one, check the actual code, does the fix handle it or not.
   Show the specific line that proves your answer, don't assert it.
5. Try to reproduce the original bug through a different path than the
   one in the bug report, using the current fixed code. If the required
   environment or safe test data is unavailable, report UNVERIFIED and
   name the exact prerequisite instead of guessing.
6. Give a final verdict: CONFIRMED FIXED, PARTIALLY FIXED (list the exact
   gap), NOT FIXED (explain what's still broken), or UNVERIFIED (state
   the prerequisite). Scope the verdict to the tested environment and
   paths. Do not soften a PARTIALLY FIXED or NOT FIXED verdict to sound
   more positive than the evidence supports.
```

## Real example output
Bug: "Rate limiter allows 101st request in the same second to succeed."
Fix: with a zero-initialized counter, incremented before checking and rejected once
`++count > 100` in `src/middleware/rateLimit.ts:22`.

Step 2: Guarantee is narrow - it only rejects when the in-memory counter for a single process exceeds 100 within the window.

Step 3: Edge cases not covered: (a) multiple server instances behind the load balancer, each with its own counter, (b) requests arriving concurrently before the counter increment resolves, (c) the counter reset logic at window boundary.

Step 4: Checked `rateLimit.ts:15-30`, the counter is a local `Map`, not shared via Redis or similar - case (a) is not handled, two instances each allow 100, so 200 requests succeed across the fleet. Case (b): the check uses `++count > 100`, so request 101 increments to 101 and is rejected; no await separates the increment and check, so single-process concurrency is actually fine.

Step 5: Reproduced by hitting two different app instances with 60 requests each inside one second - both succeeded, 120 total against a stated limit of 100.

Verdict: PARTIALLY FIXED. Single-instance bursting is fixed. Multi-instance bursting is not, the counter needs to move to shared storage (Redis) before this is safe in production.

## Why it works
The original agent's context is anchored on the bug report it was given and its own fix, which makes it a poor judge of what it missed. A pass with no attachment to the fix, instructed explicitly to disprove it, surfaces the gaps a same-context "looks good" review reliably misses - especially deployment-shape issues like multi-instance state that never show up in a local repro.
