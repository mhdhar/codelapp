---
title: "No Fix Without a Failing Test"
slug: "no-fix-without-a-failing-test"
format: "goal"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Refuse any bug fix until a test reproduces the bug, so the fix is proven, not vibes."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["shrink-the-repro-until-it-confesses", "verify-before-claiming-fixed", "root-cause-debugging-loop"]
---

## When to use this
You have a real bug report and the temptation is to let the agent jump straight to a plausible-looking patch. Use this when the bug matters enough that "seems fixed" isn't good enough: anything involving money, data integrity, or a bug that already came back once.

## The pattern
```text
There's a bug. Your goal is a fix proven by a test that failed before the fix and passes after it. Changing product code before you can reproduce the bug is not allowed.

1. Reproduce first: write the smallest test that captures the buggy behavior, using this project's existing test framework and style. Run it and paste the failing output. If you can't make it fail, stop. You haven't found the bug yet, and patching now would be guessing. Keep investigating until it fails for the reported reason.
2. Only then fix the code. Make the smallest change that turns the test green.
3. Run the new test (must pass) and the surrounding suite (must show no regressions). Paste both outputs.
4. Keep the test in the suite, named for the behavior it protects (e.g. "does not double-charge when a retry fires after success"), not "fixes bug 123".

Done means: one new test that fails on the old code and passes on the new code, suite green, both shown in real output. Anything less is not done.

Before you do anything, ask me in one message and wait: (1) the bug and how I see it, (2) how to run the tests, if it's not obvious. If I skip (2), find the test command yourself.
```

## Real example output
```
> The bug: CSV export drops the last row when the table has exactly one full page of results.

1. Repro test added to exportOrders.test.ts: "includes the final row when row count equals page size". Ran against current code:
   FAIL: expected 25 rows in CSV, got 24.
   Root cause visible in the failure: the export loop flushes a batch only when hasNextPage is true, so the final batch never flushes when totalRows % pageSize === 0.
2. Fix: flush the remaining batch after the loop in lib/export/csv.ts. 3 lines.
3. New test: PASS. Full export suite: 14/14 passing, no regressions.
4. Test kept as "includes the final row when row count equals page size". It now guards every exact-page-boundary case, not just 25.
```

## Why it works
A patch without a reproduction proves nothing: the bug may live elsewhere, or the code path may never even run. Forcing fail-then-pass pins the exact behavior, catches wrong-root-cause fixes on the spot, and leaves a permanent regression tripwire, so the same bug can't quietly return in six months.
