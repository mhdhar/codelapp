---
title: "Git Bisect Regression Hunt"
slug: "git-bisect-regression-hunt"
format: "workflow"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Automate git bisect with a real pass/fail check so the agent finds the exact breaking commit."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["probe-bisection-through-the-pipeline", "dependency-upgrade-breakage-isolation", "ci-failure-triage-loop"]
---

## When to use this
Something broke somewhere between a known-good state and now, but nobody knows which of the 40 commits in between did it, and the diff is too large to read by eye. You have (or can write) a command that reliably distinguishes good from bad.

## The pattern
```text
We have a regression. It is broken at HEAD unless I say otherwise. The
repro command I give you proves good vs. bad: it should exit 0 on good
and non-zero on bad.

Do this:

1. Run `git status` first. If the worktree is not clean, do not start
   bisect in it; ask to use a disposable worktree or wait for approval to
   preserve the existing changes safely. Record the starting branch so
   `git bisect reset` can return to it.
2. Establish the known-good ref. If I named one, use it. If I didn't,
   find one yourself: step back through history by doubling
   (HEAD~8, HEAD~16, HEAD~32, ...) running the repro command at each,
   until it passes.
3. Confirm the repro command actually fails on the bad ref and passes on
   the good ref before starting bisect. If it doesn't cleanly distinguish
   the two, stop and tell me, don't bisect on an unreliable signal.
4. Run `git bisect start`, then `git bisect bad <bad ref>` and
   `git bisect good <good ref>`.
5. At each commit git checks out, run the exact repro command (rebuild or
   reinstall dependencies first if the project requires it for that
   commit to run at all) and mark it with `git bisect good` or
   `git bisect bad` based on the real exit code, not a guess.
6. Continue until git reports the first bad commit.
7. Show me that commit's full diff (`git show <commit>`) and explain in
   plain terms what it changed that would cause this specific failure,
   tying the explanation to actual lines in the diff.
8. Run `git bisect reset` before reporting any result, including if a
   dependency install or repro fails, to return the repo to its recorded
   starting state.
9. Do not propose a fix for the regression yet unless I ask, the goal of
   this workflow is isolating the commit, not patching it.

Before step 1, ask me in one message for: (1) the repro command, or a
description of what broke so you can write one and show it to me, and
(2) a known-good ref if I know one. Wait for my reply, then start.
```

## Real example output
Repro: `npm test -- src/__tests__/pdfExport.spec.ts`. Confirmed it passes on tag `v2.14.0` and fails on `HEAD` (38 commits apart).

Bisect ran 6 steps (log2 of 38): marked commits at `a1c3f9`, `77bde2`, `f0912a` good; `9c44d1`, `2eab77` bad. Result:

```
9c44d10a4f2e3b1c8890def1234567890abcdef is the first bad commit
commit 9c44d10
Author: ...
Date:   ...
    perf: switch pdf renderer to streaming mode
    3 files changed, 41 insertions(+), 12 deletions(-)
```

Step 5: `git show 9c44d10` shows `src/lib/pdf.ts` changed `renderSync(doc)` to `renderStream(doc).pipe(res)`, and removed the `await` that previously blocked until rendering finished. The test asserts on `res.body` immediately after the render call returns, which now resolves before the stream has finished writing, since the switch to streaming made the render call return early by design. That's the regression: the test (and any caller relying on synchronous completion) needed to await the stream's `finish` event, which this commit didn't add.

## Why it works
Bisect turns a 38-commit haystack into ~6 checks by halving the search space each time, but it's only as reliable as the pass/fail signal driving it - verifying the repro command actually distinguishes good from bad before starting is what stops a bisect from confidently pointing at the wrong commit. Reading the actual diff of the isolated commit, instead of stopping at "this commit is bad," is what turns the bisect result into an explanation you can act on.
