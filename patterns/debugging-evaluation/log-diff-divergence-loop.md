---
title: "Log Diff Divergence Loop"
slug: "log-diff-divergence-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "15 min"
models: ["any"]
summary: "Compare logs from a working run and a broken run side by side to find where behavior splits."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
There's no clean stack trace, just two logs: one from a run that worked and one from a run that didn't (a good deploy vs. a bad one, staging vs. prod, before vs. after a change). You need the exact point where they stop matching, not a guess about which subsystem is at fault.

## The pattern
```text
I have two logs for the same operation. One is from a working run, one
is from a broken run. Find exactly where they diverge.

Do this:

1. Walk both logs in lockstep, line by line, matching equivalent events
   (same request ID, same operation name, same sequence position) even if
   timestamps differ.
2. Find the first line where the two logs stop matching, either one has
   an event the other doesn't, or the same event has a different value.
   Quote both lines exactly.
3. Everything before that point is common ground, do not treat it as
   suspect. Everything after it is downstream of the divergence and
   should not be treated as an independent cause.
4. Open the code responsible for producing the line that diverged. Read
   it and state what input or condition would cause it to emit the
   broken-log version instead of the working-log version.
5. If the divergent line itself doesn't explain enough, check the code
   for the last common line for a value that could differ between the
   two runs (config, environment, input payload) and trace that value
   forward into the divergent line.
6. State the root cause as: "at [log line/event], [system] did X instead
   of Y because [specific condition/value]."

Do not propose a fix based on the general area of the divergence alone,
trace it to the specific condition first.

First, ask me for both logs in one message: the working log and the
broken log, each pasted or given as a file path. If I give file paths,
read them yourself. Wait until you have both before starting.
```

## Real example output
Working log line 14: `[req_88a2] cache.get(key=user:441) -> HIT`
Broken log line 14: `[req_88a2] cache.get(key=user:441) -> MISS`

Step 2: First divergence is this cache lookup, both logs match exactly through line 13 (session validation, permission check).

Step 4: Read `src/lib/cache.ts:40`, `cache.get` builds the key as `` `user:${id}` ``. In the broken run, `id` is a number `441`; in the working run it's a string `"441"` from an older client version, both should produce the same key.

Step 5: Checked `redis.ts:12`, the client was upgraded to v5 last week (`package.json` diff), and v5's `get` no longer coerces numeric keys to strings before hashing, per the client's changelog. So `user:441` (from string 441) and `user:441` (from number 441) hash to different cache slots under v5 even though they render identically in the log.

Step 6: Root cause: at the cache.get call in `cache.ts:40`, the Redis client returns MISS instead of HIT because the v5 client hashes numeric-typed keys differently than string-typed keys, and `id` is a number for newer clients, causing a cache miss and the subsequent slow path that broke the request.

## Why it works
Anchoring on the first point of divergence keeps the investigation from wandering into unrelated log noise after the actual break, and stops "everything after the crash looks wrong" from being treated as ten separate bugs. Tracing the divergent value back to its source, instead of stopping at "cache missed," is what catches a root cause (a type change interacting with a client upgrade) that no single log line states outright.
