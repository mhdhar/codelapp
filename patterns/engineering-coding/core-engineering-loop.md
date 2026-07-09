---
title: "The Core Engineering Loop"
slug: "core-engineering-loop"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "A six-step loop for any coding task: clarify, check, choose, execute, verify, recommend."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You're about to hand an AI agent a real engineering task and want it to stop guessing. Use this as the default operating loop for any nontrivial change, not just bug fixes.

## The pattern
```text
Work the task at the end of this message using a strict six-step loop. Do not skip a step or merge steps.

1. CLARIFY: Restate the ask in your own words. List any assumptions you're making. If anything is ambiguous, ask me before proceeding instead of guessing.
2. CHECK: Read the existing code, config, and tests related to this task. Report what's already there before proposing changes. Do not assume something is missing until you've looked.
3. CHOOSE: Propose the smallest approach that solves the task. If there are two reasonable approaches, list both with tradeoffs and pick one, stating why.
4. EXECUTE: Make the change. Touch only files required for this task.
5. VERIFY: Run the actual build, tests, or the specific failing case. Show me the real output. Do not say "this should work" without running it.
6. RECOMMEND: State what you did in one line, what evidence proves it works, and the one next step (if any) I should take.

Stop after step 6 and wait for my go-ahead before starting anything not in the original task.

The task:
```

## Real example output
```
1. CLARIFY: You want rate limiting added to POST /api/upload, capped at 5 requests/min per user. Assuming per-user means per authenticated user ID, not per IP. Confirm?
   > Yes, per user ID.
2. CHECK: No existing rate limiter in the repo. middleware/auth.ts already resolves req.user. package.json has no rate-limit library installed.
3. CHOOSE: Using in-memory token bucket via `rate-limiter-flexible` (already common for single-instance deploys). Redis-backed would be more correct for multi-instance but this app runs on one instance, so skipping that complexity.
4. EXECUTE: Added middleware/rateLimit.ts, wired into routes/upload.ts.
5. VERIFY: Ran `npm test rateLimit`, 4/4 passing. Manually hit the endpoint 6x in a loop, 6th request returned 429 as expected.
6. RECOMMEND: Rate limiting is live on upload only. Next step: apply the same middleware to POST /api/comments if you want it there too.
```

## Why it works
Forcing clarify and check before choose kills the two most common failure modes: solving the wrong problem and duplicating code that already exists. Ending on verify plus a one-line recommendation stops the agent from claiming success on vibes.
