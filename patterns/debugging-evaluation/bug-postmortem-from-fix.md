---
title: "Bug Postmortem From a Fix"
slug: "bug-postmortem-from-fix"
format: "workflow"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Turn a fixed bug into a structured postmortem so the same class of bug doesn't ship again."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
A real bug (not a typo, an actual incident or user-facing break) just got fixed and you're about to move on. You want a short, honest writeup while the context is fresh, not two weeks from now when nobody remembers which assumption was wrong.

## The pattern
```text
We just fixed this bug: [BUG DESCRIPTION]
The fix that was applied: [DIFF, PR LINK, OR SUMMARY OF THE CHANGE]

Write a postmortem using the actual diff and any logs/tickets from this
conversation, not a generic template filled with guesses. Structure:

1. What broke: one or two sentences, plain language, what the user or
   system actually experienced.
2. Impact: how many users/requests/how long, if that data exists in what
   we've discussed. If it's unknown, say "impact not measured" instead of
   inventing a number.
3. Root cause: one sentence, tied to the specific line(s) of code or
   config that were wrong. Not "a bug in the payment logic," name the
   actual mechanism.
4. Why it wasn't caught earlier: look at whether a test, type check,
   lint rule, or code review comment could plausibly have caught this,
   and say specifically which layer failed to catch it. If nothing
   reasonably could have caught it, say so instead of inventing blame.
5. The fix: what changed, in one or two sentences.
6. Prevention: one concrete, specific action (a new test, a lint rule, a
   type change, a monitoring alert) that would catch this class of bug
   next time. Not "be more careful" or "add more tests" without naming
   which test.

Keep the whole thing under 300 words. Do not include speculation labeled
as fact, mark anything inferred as "likely" or "unconfirmed."
```

## Real example output
**What broke**: Users on the annual billing plan were charged the monthly price when they upgraded mid-cycle.

**Impact**: 12 accounts affected between the deploy on Tuesday and the fix Thursday, per the billing_events table filtered on `plan_change_type = 'upgrade'` in that window. All 12 were refunded the difference manually.

**Root cause**: `src/billing/prorate.ts:52` computed the prorated amount using `plan.monthlyPrice` unconditionally, instead of branching on `plan.interval`, so annual-plan upgrades were prorated against the wrong base price.

**Why it wasn't caught earlier**: `prorate.spec.ts` only had test cases for monthly-to-monthly upgrades, there was no test case for an annual-plan upgrade, so the branch gap was never exercised. Code review approved the PR without a reviewer noticing the missing `interval` check, likely because the diff's focus was the new upgrade UI, not the pricing math.

**The fix**: added an `if (plan.interval === 'annual')` branch in `prorate.ts:52` that prorates against `plan.annualPrice / 12`, matching the existing monthly branch's pattern.

**Prevention**: added `prorate.spec.ts` cases for annual-to-annual and monthly-to-annual upgrades (2 new cases), and added a note to the PR template asking "does this touch billing math? confirm both plan intervals are tested" for the billing directory specifically.

## Why it works
Naming the exact layer that should have caught the bug (missing test case vs. missing lint rule vs. review gap) turns "we'll be more careful" into an action someone can actually implement and check off. Requiring the root cause to cite a real line, and impact to cite real data or say "unmeasured," keeps the postmortem from turning into comfortable fiction once the pressure of the incident has passed.
