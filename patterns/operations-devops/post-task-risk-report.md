---
title: "Post-Task Risk & Next-Action Report"
slug: "post-task-risk-report"
format: "loop"
category: "operations-devops"
tools: ["universal"]
difficulty: "beginner"
est_time: "4 min"
models: ["any"]
summary: "End every task with what changed, what's verified, what's risky, and one clear next move."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["session-handoff-note", "decision-log-append", "rollback-readiness-check"]
---

## When to use this
You want a consistent close-out after every task in a session, not just at the end of the day, so you always know exactly what to trust, what's still shaky, and what to do next instead of getting a vague "let me know what you'd like to do."

## The pattern
```text
Whenever you finish a task in this session (not just at the end of the
session), report back in this exact structure before moving on to anything
else:

1. What changed: bullet list of files/behavior touched, one line each.
2. What was verified: the actual command(s) you ran and their real output
   (test run, manual check, build). "Should work" is not verification.
3. What risk remains: anything not covered by the verification above:
   untested edge cases, assumptions made, code paths not exercised.
   If there is truly no remaining risk, say so explicitly, don't omit
   the section.
4. Recommended next step: pick exactly one: keep working, test more,
   commit, open a PR, merge, deploy, or keep this separate from other work.
   Don't list options, pick one.

Keep it under 10 lines total. Don't pad it with restating the original
request. If a step genuinely doesn't apply (e.g. no tests exist yet), say
that instead of skipping the section silently.
```

## Real example output
```
What changed:
- src/api/webhooks/stripe.ts: added signature verification before
  processing checkout.session.completed
- src/api/webhooks/stripe.test.ts: added test for invalid signature -> 400

What was verified: npm test -- webhooks -> 6 passed, 0 failed. Manually
POSTed a payload with a tampered signature to the local dev server, got
400 as expected.

What risk remains: only tested the checkout.session.completed event type.
The refund and dispute webhook handlers use the same verification function
but weren't individually re-tested after this change. Should be covered
by the same fix, but not directly confirmed.

Recommended next step: commit this now, then run the full webhook test
suite (not just the filtered one) before touching refund/dispute handlers.
```

## Why it works
Separating "verified" from "risk remains" forces you to name the untested part instead of letting a green test run imply more coverage than it actually has. A single recommended action, not a menu of options, stops the session from drifting into ambiguous limbo at the exact moment you'd otherwise lose track of what's next.
