---
title: "Port and Process Squatter Sweep"
slug: "port-and-process-squatter-sweep"
format: "prompt"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Find every dev server, stray node process, and container squatting on your ports before you debug the wrong app."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["parallel-worktree-risk-audit", "assumption-audit-before-debugging", "env-var-drift-audit"]
---

## When to use this
Port 3000 is "already in use", your app is serving stale code, or hot reload stopped working. You've been running several projects and worktrees and you no longer know which processes on this machine are yours, current, and needed.

## The pattern
```text
Something on this machine is squatting on my dev ports and I've lost track
of what's actually running. Do a read-only sweep and report:

1. List every listening TCP port with its owning process
   (`lsof -iTCP -sTCP:LISTEN -P -n` on macOS/Linux, or the platform
   equivalent). For each PID, get the command and its working directory.
2. If Docker is installed, list running containers and their published
   ports too.
3. For each port in use, name the project it belongs to, judged by the
   process's working directory. "Unknown" is an acceptable answer;
   guessing is not.
4. Flag suspects:
   - a dev server whose working directory is not the project we're in now
   - two or more processes serving the same codebase
   - anything older than 24 hours (check elapsed time in `ps`)
   - orphaned node/python processes with no terminal attached
5. Output a table: port | PID | command | project dir | age | verdict.
   Verdict is KEEP or SUSPECT with a one-line reason.
6. Kill nothing. End with the exact kill command for each SUSPECT row so
   I can run them myself.
```

## Real example output
```
| Port  | PID   | Command            | Project dir                  | Age    | Verdict |
|-------|-------|--------------------|------------------------------|--------|---------|
| 3000  | 41283 | next-server (v15)  | ~/projects/masjidos          | 26h    | SUSPECT: stale, we're working in codel-site |
| 3001  | 88410 | next-server (v15)  | ~/projects/codel-site        | 15m    | KEEP: current project, auto-bumped off 3000 |
| 5173  | 79002 | node (vite)        | ~/worktrees/melbourne        | 3d     | SUSPECT: worktree merged Monday |
| 54321 | 1200  | docker: supabase   | container supabase_db_local  | 2d     | KEEP: shared local DB |
| 8080  | 90331 | python3 -m http... | ~/Downloads                  | 6d     | SUSPECT: forgotten file server, no terminal attached |

Why port 3000 was "in use": the masjidos dev server from yesterday.
Kill commands: kill 41283 && kill 79002 && kill 90331
```

## Why it works
Half of "my dev server is broken" is really "the wrong dev server is running", and the working-directory column is what exposes that in seconds. Keeping the sweep read-only, with kill commands handed to you instead of run for you, makes it safe to fire off any time something feels off.
