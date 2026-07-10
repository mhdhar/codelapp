---
title: "Flaky Test Triage Loop"
slug: "flaky-test-triage-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Re-run a failing test enough times to tell a real regression from noise before you touch any code."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
aliases: ["flaky tests", "intermittent test failures"]
related: ["ci-failure-triage-loop", "git-bisect-regression-hunt", "verify-before-claiming-fixed"]
---

## When to use this
A test failed in CI or locally and the instinct is to either dismiss it as "probably flaky" or immediately start editing code. Both are guesses. You want an actual pass rate before deciding whether this is a real regression, timing noise, or a bad test.

## The pattern
```text
A test failed. Do not change any application code yet. First triage:

1. Run the exact same test command 10 times in a row, back to back, with
   no changes in between. Record pass/fail for each run.
2. Report the pass rate (e.g. "7/10 passed") and paste the failure output
   from each failed run, not just the first one.
3. If it failed 10/10 with the same error every time: treat this as a
   real, consistent failure. Stop here and hand it to root-cause
   debugging, this is not a flakiness problem.
4. If it passed at least once and failed at least once: look for the
   actual source of nondeterminism in the test itself, don't assume
   "flaky" and move on. Check for: unmocked timers/dates, real network
   or filesystem calls, shared state between tests (a DB row, a global,
   a port), test order dependence, or a race between an async operation
   and an assertion that doesn't await it.
5. State which of those causes the evidence points to, quoting the
   specific line in the test that creates the nondeterminism.
6. Propose a fix to the TEST (not the application code) unless your
   evidence from step 5 shows the application itself has a real race
   condition. If it's a genuine application race condition, say so
   explicitly and treat it as a real bug, not a quarantine candidate.
7. After the fix, re-run 10 times again and report the new pass rate.
   Only call it resolved at 10/10.

Before step 1, ask me for the failing test (name or exact command) and
wait for my reply. If I give a name rather than a command, work out the
exact command to run just that test in this repo and show it to me.
```

## Real example output
Test: `checkout.integration.spec.ts::"applies discount before tax"`

Step 1-2: Ran 10x. Pass rate 6/10. Failures 2, 5, 7, 9 all show the same assertion error: expected `discountApplied: true`, got `discountApplied: false`.

Step 4: Test calls `await applyDiscount(cart)` then immediately asserts on `cart.discountApplied` without awaiting the webhook callback that `applyDiscount` fires to update the cart asynchronously (`discountService.ts:61`, fire-and-forget, no returned promise).

Step 5: Nondeterminism is a race between the test's assertion and the async webhook callback, timing-dependent, not related to checkout logic itself.

Step 6: This is a test problem, not an app problem - production users never observe partial state because the UI subscribes to the cart store and re-renders when the webhook resolves, whatever the delay. Fixed the test to `await waitForCartUpdate(cart)` before asserting.

Step 7: Re-ran 10x, 10/10 passed.

## Why it works
Measuring the real pass rate before touching anything stops two failure modes at once: dismissing a real regression as "just flaky," and wasting an hour bisecting application code for a bug that only exists in an unawaited test assertion. The rule to fix the test unless there's evidence of a real app race keeps quarantine decisions evidence-based instead of convenient.
