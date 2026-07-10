---
title: "Session Handoff Note for a Different Session or Teammate"
slug: "session-handoff-note"
format: "workflow"
category: "operations-devops"
tools: ["universal"]
difficulty: "intermediate"
est_time: "8 min"
models: ["any"]
summary: "Write a handoff so the next session or teammate starts working in minutes, not re-explaining."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["project-state-handoff-file", "repomix-context-handoff", "end-of-session-git-checklist"]
---

## When to use this
You're stopping mid-task, work isn't finished, and either you in a new session or a teammate needs to pick it up cold. A vague commit message isn't enough context to continue safely.

## The pattern
```text
I'm stopping mid-task and need a handoff note so the next session or a
teammate can pick this up cold, with zero re-explaining. Do the following:

1. Run `git status` and `git branch --show-current`. Note the branch name and
   whether there are uncommitted changes.
2. If there are uncommitted changes that are safe to keep, commit them to the
   current branch with a message prefixed "WIP:". If they are experimental or
   half-broken, stash them instead and note the stash name.
3. Push the current branch to the remote.
4. Write a handoff note with these exact sections:
   - Goal: what this branch/task is trying to accomplish, one sentence.
   - Done: bullet list of what's working and verified.
   - In progress: what's partially done and exactly where it's incomplete
     (file + function name, not just "the API part").
   - Next step: the single next action to take, specific enough to start
     coding immediately.
   - Known issues: anything broken, flaky, or deliberately left unfinished.
   - How to verify: the exact command(s) to run to confirm current state
     (tests, build, dev server).
5. Output the note as plain text in the response. Do not save it as a file
   unless I explicitly ask.
```

## Real example output
```
Branch: feature/csv-export (pushed, working tree clean, 1 stash: "wip-streaming-attempt")

Goal: Add CSV export for the orders table with streaming for large datasets.

Done:
- Basic CSV export endpoint at POST /api/orders/export (non-streaming, works up to ~5k rows)
- Column selection UI in ExportDialog.tsx
- Tests for the non-streaming path pass

In progress:
- Streaming export using ReadableStream in src/api/orders/export/route.ts,
  stalls after ~10k rows, likely backpressure not handled in the chunk writer
  (see writeChunk() around line 88). Stashed as "wip-streaming-attempt", not
  committed because it breaks the non-streaming fallback.

Next step: Pop the stash, fix backpressure handling in writeChunk(), test
with the 50k-row fixture in test/fixtures/orders-large.csv.

Known issues: Non-streaming export has no row limit guard, will OOM above
~20k rows if streaming isn't finished by launch.

How to verify: npm test -- export, then npm run dev and hit
POST /api/orders/export with test/fixtures/orders-small.csv.
```

## Why it works
Handoffs fail when the next person has to reverse-engineer state from a diff and a vague commit message. Naming the exact file, function, and failure mode turns "figure out where I left off" into "start at line 88."
