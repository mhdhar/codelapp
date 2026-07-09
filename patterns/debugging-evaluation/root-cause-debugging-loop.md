---
title: "Root Cause Debugging Loop"
slug: "root-cause-debugging-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Force one hypothesis at a time against real evidence, so the AI stops guess-and-check fixing."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
A bug is reported but the AI jumps straight to a fix without reproducing it. You've seen this fail before: the fix doesn't stick because nobody confirmed the actual cause.

## The pattern
```text
We have a bug.

Follow this loop and do not skip steps:

1. Reproduce the bug first. Write or run a test/command that fails in the
   exact way described. Paste the real failing output. If you cannot
   reproduce it, stop and tell me what you tried.
2. State exactly one hypothesis for the root cause, based only on the
   evidence you've seen so far (code, logs, output). Do not list multiple
   possible causes.
3. Find evidence that confirms or rejects that hypothesis before writing
   any fix. Read the relevant code, add a log line, or run a targeted
   check. Show me the evidence.
4. If the hypothesis is wrong, discard it and go back to step 2 with a new
   one. Do not carry forward assumptions from the rejected hypothesis.
5. Only once you have confirmed the root cause with evidence, propose the
   fix and explain why it addresses the confirmed cause, not just the
   symptom.
6. Re-run the original failing reproduction from step 1 and paste the new
   output to prove the fix works.

Do not guess. Do not propose a fix before completing step 3.

If this conversation already contains the bug report, use it and start
now. If not, first ask me to describe the bug and how I see it, then
wait for my reply.
```

## Real example output
Step 1: Reproduced with `npm test -- checkout.spec.ts`, failing assertion: expected total 42.50, got 45.00.
Step 2: Hypothesis: tax is applied twice, once in `calculateTotal` and once in the checkout summary component.
Step 3: Read `calculateTotal` (src/lib/pricing.ts:44), it does not apply tax. Read `CheckoutSummary.tsx:88`, confirmed it adds tax on top of a total that already includes it upstream in `useCart.ts:120`. Evidence confirms hypothesis.
Step 5: Removed the duplicate tax addition in `CheckoutSummary.tsx:88`.
Step 6: Re-ran `npm test -- checkout.spec.ts`, all 6 tests pass, total now 42.50.

## Why it works
Forcing one hypothesis at a time with evidence stops the AI from pattern-matching a plausible-looking fix onto the wrong cause. Reproducing first means the "fix" gets checked against the same failure that started the loop, not a new assumption.
