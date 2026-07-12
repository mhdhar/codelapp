---
title: "Shrink the Repro Until It Confesses"
slug: "shrink-the-repro-until-it-confesses"
format: "goal"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Cut a failing case down until every remaining line is load-bearing and the bug has nowhere left to hide."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["git-bisect-regression-hunt", "no-fix-without-a-failing-test", "probe-bisection-through-the-pipeline"]
---

## When to use this
A big test, script, or input file fails, and the failure could be caused by any of a hundred moving parts inside it. Reading the whole thing gets you nowhere. You want the smallest version that still fails, because in that version the cause is usually visible on sight.

## The pattern
```text
I have a failing repro. Your goal is not to fix it yet. Your goal is
to shrink it.

Success criteria, all three must hold:
- The reduced version still fails with the same failure signature (same
  error type and relevant assertion or normalized message, not just "it
  fails"). Normalize volatile timestamps, IDs, and ports before comparing.
- Removing any single remaining piece (a line, an input field, a setup
  step) either makes it pass or changes the failure signature. Every
  survivor is proven load-bearing.
- It fits on one screen where practical (about 20 lines of code or input).
  If a longer repro is necessary, say why instead of hiding a required piece.

Method:
1. Run the repro once, unchanged. Record the exact failure signature.
   This is your reference, compare every later run against it word for
   word.
2. Copy the repro into a scratch file. Never shrink the original.
3. Cut aggressively: delete half the setup, inline the helpers, replace
   real data with the smallest literal that keeps the failure. Re-run
   after every cut.
4. If a cut makes it pass or changes the signature, put that piece back.
   It is load-bearing, mark it as such.
5. Repeat until every remaining piece has survived a removal attempt.
6. Show me the final reduced repro, the run count, the before/after
   line counts, and what the survivors have in common. Only then, if
   the cause is now obvious, name it.

Do not skip re-running after a cut because it "obviously" doesn't
matter. Cheap runs are the whole method.

First, ask me for the failing repro (test name, command, or file path),
then wait. Don't start cutting until you have it.
```

## Real example output
Round 0: `tests/export.integration.spec.ts` fails at 412 lines, signature `TypeError: Do not know how to serialize a BigInt`.

18 reduction rounds, 21 runs total. Cuts that survived reversal: the `order.total` field, the `JSON.stringify` call, nothing else. Final scratch repro, 6 lines:

```
const order = await db.order.findFirst();
JSON.stringify(order);
```
plus 4 lines of DB setup. 412 lines -> 6 lines, identical signature on every failing run.

Survivors have one thing in common: `order.total` is a Prisma `Decimal` column that the new driver returns as `BigInt`. Cause is visible on sight: the export path serializes raw DB rows, and the driver upgrade changed the runtime type of `total`. Fix belongs in the serializer, not the test.

## Why it works
Shrinking is a proof, not a cleanup: each removal attempt is an experiment, and every line still standing at the end has demonstrated it participates in the failure. The bug has to live in the intersection of the survivors, which is usually small enough to read in one glance. As a bonus, the reduced case is a ready-made regression test.
