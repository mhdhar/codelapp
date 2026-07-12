---
title: "Parallel Worktree Risk Audit"
slug: "parallel-worktree-risk-audit"
format: "loop"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "advanced"
est_time: "15 min"
models: ["any"]
summary: "Sweep every worktree before touching any of them, and label each safe or risky."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["end-of-session-git-checklist", "port-and-process-squatter-sweep", "destructive-cleanup-blast-radius-check"]
---

## When to use this
You've got several worktrees or branches open at once (Conductor-style, one per feature) and you've lost track of which ones have uncommitted work, mid-rebase state, or stashes you can't afford to lose. Run this before acting on any of them.

## The pattern
```text
I have multiple git worktrees/branches in flight and need a risk audit
before I touch any of them. Audit every worktree this repo knows about
(start from `git worktree list` here; if my worktrees live under a
different root, I'll name that directory in my next message):

1. List every worktree with `git worktree list`, and for each one:
   - Current branch name and whether it's tracking a remote.
   - `git status --short` output (uncommitted/untracked files).
   - How far ahead/behind its upstream it is, when an upstream exists
     (`git rev-list --left-right --count <branch>...@{upstream}`). If it has
     no upstream, mark it `NO UPSTREAM` instead of comparing it to an assumed
     `origin/<branch>`.
   - Whether it has any stashes (`git stash list` run inside that worktree).
2. Classify each worktree as one of:
   - SAFE TO TOUCH: clean or only local commits, nothing shared depends on it.
   - CAUTION: uncommitted or stashed work exists; local edits are fine,
     do not push/merge/delete without asking.
   - DO NOT TOUCH: mid-operation (unresolved merge/rebase, detached HEAD,
     or a rebase/cherry-pick in progress).
3. Output a table: worktree path | branch | status | classification | reason.
4. If any worktree is DO NOT TOUCH, name it first and stop there; don't
   move on to suggesting actions on the others until I've seen that one.
5. Do not run any push, merge, branch delete, or `git worktree remove` as
   part of this audit. This is read-only reconnaissance.
```

## Real example output
```
git worktree list: 4 worktrees under ~/projects/masjidos-worktrees/

| Path        | Branch                   | Status                           | Classification | Reason |
|-------------|--------------------------|-----------------------------------|-----------------|--------|
| rabat/      | feature/prayer-times-api | clean, 2 ahead of origin          | SAFE TO TOUCH   | Local commits only, nothing pushed yet |
| melbourne/  | feature/admin-dashboard  | 3 modified, 1 untracked           | CAUTION         | Uncommitted UI work, no stash |
| nagoya/     | fix/timezone-bug         | 1 stash ("wip-dst-edge-case")     | CAUTION         | Stashed experiment, don't discard |
| des-moines/ | main                     | rebase in progress (REBASE_HEAD)  | DO NOT TOUCH    | Interrupted rebase, needs manual resolve first |

DO NOT TOUCH: des-moines/ has an interrupted rebase (REBASE_HEAD present).
Resolve that before I take any action there. The other three are safe for
local work; melbourne/ and nagoya/ need your OK before anything leaves them.
```

## Why it works
Auditing all worktrees in one read-only pass before acting on any of them catches the one mid-rebase or half-merged worktree before an unrelated command in a different worktree accidentally collides with it. Classifying instead of just listing forces a safe/risky decision up front, not mid-action.
