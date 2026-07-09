---
title: "Engineering Finish Report"
slug: "engineering-finish-report"
format: "workflow"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "End every completed task with a fixed report: what changed, what's verified, what's risky, next step."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
A task is fully done (not interrupted mid-way) and you need a decision-ready summary before deciding whether to commit, open a PR, merge, or deploy. Use this as the closing step on any completed change, not just big ones.

## The pattern
```text
The task we've been working on in this session is complete. Before I decide what to do with it, give me a Finish Report in exactly this structure. Start by restating, in one line, what the task was supposed to get done.

1. WHAT CHANGED: List the files touched and, in one line each, what changed in them.
2. WHAT WAS VERIFIED: The exact command(s) you ran and their real output (not "should work"). If nothing was verified, say so explicitly — do not imply it was tested if it wasn't.
3. WHAT RISK REMAINS: Anything not covered by the verification above — edge cases, untested paths, assumptions that could be wrong. If genuinely none, say "no known risk," don't invent one.
4. RECOMMENDED NEXT STEP: The single most useful thing to do next.
5. ROUTING: Recommend exactly one of: keep local and iterate more / commit only / open a PR / merge / deploy. State why that one and not a more aggressive option.
```

## Real example output
```
The task: Add pagination to GET /api/orders (previously returned all rows).

1. WHAT CHANGED:
   - routes/orders.ts: added `page` and `limit` query params, default limit 25, max 100.
   - lib/db/orderQueries.ts: added LIMIT/OFFSET to the base query.
   - routes/orders.test.ts: added 3 tests for default, custom, and over-max limit.

2. WHAT WAS VERIFIED: ran `npm test orders` — 9/9 passing. Manually hit GET /api/orders?page=2&limit=10 against local dev DB (312 seeded rows), got 10 rows starting at offset 10, matches expected slice.

3. WHAT RISK REMAINS: total row count isn't returned in the response yet, so the frontend can't build "page 3 of 12" style UI off this alone. Not tested against the production DB's actual row count (312k, vs 312 in dev), so OFFSET performance at scale is unverified.

4. RECOMMENDED NEXT STEP: add a `total` count to the response before wiring up frontend pagination controls.

5. ROUTING: commit only, don't open a PR yet — the response shape is likely to change once `total` is added, better to land both together in one reviewable diff.
```

## Why it works
A fixed structure stops "done" from meaning five different things depending on the day. Splitting verified fact from remaining risk keeps the agent honest about the gap between them, and forcing a single routing recommendation turns the report into a decision you can act on in ten seconds.
