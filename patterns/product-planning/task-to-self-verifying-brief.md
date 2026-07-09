---
title: "Turn a One-Line Task into a Self-Verifying Brief"
slug: "task-to-self-verifying-brief"
format: "workflow"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Expand a one-liner into success criteria and a verify step so the agent can loop without you babysitting it."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You typed a task in one line ("fix the flaky login test", "add rate limiting") and you're tempted to just hit enter. Do this first instead, it takes two minutes and means the agent checks its own work instead of pinging you every few steps.

## The pattern
Step 1, expand the task:
```text
Expand the one-line task I give you into a brief before doing any work.

Write:
1. Restated goal, one sentence.
2. Success criteria: a checklist of specific, testable conditions that must be true when this is done. No vague criteria like "works correctly", every item must be something you can run or check.
3. Verify step: the exact command(s) or manual check you'll run to confirm each success criterion, before declaring this done.

Then execute the task, and loop on your own verify step until every criterion passes. Report the final checklist with pass/fail for each item.

First ask me for the one-line task, then wait. Don't start until you have it.
```

Step 2, if a criterion fails, don't ask for help immediately, just say:
```text
Keep looping: fix the failing criterion, re-run the verify step, repeat until all pass or you hit the same failure twice in a row. If you hit it twice, stop and report what you tried.
```

## Real example output
"GOAL: Add rate limiting to the /api/login endpoint.

SUCCESS CRITERIA:
- [ ] Requests over 5/min from one IP return 429
- [ ] Requests under the limit succeed normally
- [ ] Existing login tests still pass
- [ ] Rate limit resets after the window (60s)

VERIFY STEP: run `npm test tests/rateLimit.test.ts`, plus a manual curl loop of 6 requests checking the 6th returns 429.

RESULT: 4/4 criteria passing. Ran verify step twice, second run confirms reset behavior after 60s wait."

## Why it works
Testable criteria give the agent something concrete to check itself against instead of guessing when it's "probably done." The verify step turns a vague task into a loop with a real exit condition, so it can keep iterating on its own instead of surfacing every intermediate state back to you.
