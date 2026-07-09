---
title: "Destructive Cleanup Blast-Radius Check"
slug: "destructive-cleanup-blast-radius-check"
format: "loop"
category: "operations-devops"
tools: ["universal"]
difficulty: "advanced"
est_time: "6 min"
models: ["any"]
summary: "Force a recoverability check before any delete, drop, or force-push touches real state."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to run a command that destroys data or history — `rm -rf`, `DROP TABLE`, `git branch -D`, `git worktree remove --force`, `git push --force` — where a mistake isn't fixable by just re-running the last command.

## The pattern
```text
Before running any command that deletes, drops, force-overwrites, or
otherwise destroys data or history — including but not limited to `rm -rf`,
`DROP TABLE`/`TRUNCATE`, `git branch -D`, `git worktree remove --force`,
`git push --force`, or a migration that removes a column — stop and run
this check instead of the command:

1. Name the exact command and its target.
2. Answer: is this recoverable if it turns out to be wrong, and how?
   - Recoverable (e.g. reflog, trash, a known backup exists) — name the
     specific recovery path.
   - Not recoverable (e.g. no backup, force-push overwrites shared history,
     production data with no snapshot) — say so plainly.
3. If NOT RECOVERABLE: stop, state that clearly, and ask for explicit
   confirmation before running it. Do not proceed on an assumed yes.
4. If RECOVERABLE: state the recovery path in your report, then proceed.
5. For anything touching a shared or production surface (a shared branch,
   a shared database, anything other people rely on), treat it as NOT
   RECOVERABLE regardless of technical reversibility. The blast radius
   includes other people, not just data. When in doubt about whether
   something is shared, treat it as shared.
```

## Real example output
```
Command: DROP TABLE staging_import_backup;
Target: staging_import_backup table, Supabase project "masjidos-staging"

Recoverability: RECOVERABLE. Point-in-time recovery is enabled on this
project with a 7-day window, and this table hasn't been written to in 11
days — a PITR restore to any point in the last week would bring it back if
needed. Also confirmed no other table has a foreign key into it.

Proceeding: dropped staging_import_backup. Table removed, no errors.

---

Command: git push --force origin main
Target: main branch, shared remote

Recoverability: NOT RECOVERABLE by policy — this touches a shared branch.
Reflog technically covers it locally, but two other people have main
checked out, and a force-push would silently rewrite their history too.

Stopping. Do not run this without your explicit confirmation — and
recommend `git push --force-with-lease` at minimum if you do confirm.
```

## Why it works
Naming the recovery path before running the command turns "is this safe" into a specific, checkable answer instead of a gut feeling. Treating anything shared as not-recoverable by policy — even when it's technically reversible — accounts for the actual risk: other people's state, not just yours.
