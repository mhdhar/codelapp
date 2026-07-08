---
title: "Clean Session Handoff"
slug: "clean-session-handoff"
format: "workflow"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "End a coding session in a state the next session (or agent) can pick up without re-discovering context."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You're stopping mid-task, whether the session is ending, you're switching machines, or you're handing the work to a different agent or teammate. Use this before you close the terminal, not after you've already lost track of what's uncommitted.

## The pattern
```text
We're ending this session mid-task. Before we stop, do the following:

1. Run git status and git diff. Tell me exactly what's staged, unstaged, and untracked.
2. For each uncommitted change, tell me whether it's finished, half-done, or experimental/should be discarded.
3. Commit anything finished with a clear message. Do not bundle unrelated changes into one commit.
4. For anything half-done, leave it uncommitted but write a one-paragraph note titled "SESSION HANDOFF" covering:
   - What the current task is
   - What's done so far
   - What's broken or incomplete right now
   - The exact next step to take, including file names
5. Save that note as [HANDOFF FILE PATH, e.g. HANDOFF.md or a comment in the relevant file].
6. Confirm the working tree is either clean or clearly annotated, and tell me if there's anything I need to decide before the next session starts.
```

## Real example output
```
git status: modified src/api/checkout.ts, src/api/checkout.test.ts (unstaged). New file src/lib/discountEngine.ts (untracked).

- checkout.ts: discount logic wired in, finished, tests pass locally.
- discountEngine.ts: core logic done, but percentage-off codes aren't handled yet, only flat-amount codes.
- checkout.test.ts: added tests for flat-amount codes only.

Committed: "Add flat-amount discount codes to checkout" (checkout.ts, checkout.test.ts, discountEngine.ts).

SESSION HANDOFF (saved to HANDOFF.md):
Task: add discount code support to checkout.
Done: flat-amount codes work end to end, tests passing (npm test discount, 6/6 green).
Incomplete: percentage-off codes are not implemented. discountEngine.ts has a TODO at line 34 where the percentage branch needs to go.
Next step: implement the percentage branch in applyDiscount() in discountEngine.ts, then add matching tests in checkout.test.ts.

Working tree is clean. Nothing pending, no decision needed before next session.
```

## Why it works
The next session, whether it's you tomorrow or a fresh agent with zero memory, wastes its first ten minutes reconstructing state if you don't write it down. A committed, working slice plus an explicit note on the unfinished slice means the next start is "read one paragraph and continue," not "re-run git blame and guess."
