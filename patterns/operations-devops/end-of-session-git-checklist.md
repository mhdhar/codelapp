---
title: "End-of-Session Git Checklist"
slug: "end-of-session-git-checklist"
format: "workflow"
category: "operations-devops"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Run before closing any coding session so nothing sits uncommitted overnight."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["session-handoff-note", "parallel-worktree-risk-audit"]
---

## When to use this
You're wrapping up for the day or handing off a session. You want a clean git state, not a pile of uncommitted work waiting to get lost or conflict with tomorrow's changes.

## The pattern
```text
Before we end this session, run the end-of-session checklist:

1. Run `git status` and list every modified, staged, and untracked file.
2. For each change, propose: commit it, stash it, discard it, or leave it
   untouched because it belongs to another task. Ask me to approve every
   stash, discard, commit, or push action before running it.
3. Propose logical commits with clear messages. Do not bundle unrelated
   changes into one commit.
4. Run the project's test or build command if one exists. Report the result
   before asking to commit.
5. After approval, perform only the approved commit, stash, discard, or push
   actions, staging only the intended paths.
6. Run `git status` again and report the actual remaining state. Do not claim
   the tree is clean if intentionally untouched files remain.
7. Write a one-line session note: what got done this session, and what the very next step is. Output it as plain text, do not save it to a file unless I ask.

Do not skip a step because it seems obvious. If something can't be completed (tests fail, push is rejected, merge conflict), stop and tell me instead of working around it silently.
```

## Real example output
```
git status: 4 modified, 1 untracked
- src/auth/session.ts (modified) -> commit: "fix session token refresh race"
- src/auth/session.test.ts (modified) -> same commit
- README.md (modified) -> separate commit: "docs: note new SESSION_TTL env var"
- .env.local.example (untracked) -> add to same docs commit
- debug.log (untracked) -> discarded, added to .gitignore

Tests: 42 passed, 0 failed.
Commits created: 2. Pushed to origin/feature/session-refresh.
git status: working tree clean, up to date with origin/feature/session-refresh.

Session note: Fixed session token refresh race condition and documented
SESSION_TTL. Next: add integration test for concurrent refresh requests.
```

## Why it works
A fixed checklist run every time removes the judgment call of "is this worth committing" at the exact moment you're most likely to skip it, right before closing the laptop. The final `git status` check is the actual proof, not a guess.
