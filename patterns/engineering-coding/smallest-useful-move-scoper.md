---
title: "Smallest Useful Move Scoper"
slug: "smallest-useful-move-scoper"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Cut an oversized task into ordered slices and execute only the first, with the rest explicitly deferred."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
A task arrives already bloated, like "redo the dashboard" or "clean up the auth system," and starting to code immediately risks a sprawling, hard-to-review change. Use this before touching any file, when the task as stated is bigger than one sitting should be.

## The pattern
```text
The task at the end of this message is probably too big to do in one clean pass. Scope it down before writing code.

Loop:
1. Break the task as given into an ordered list of slices, each one independently shippable (the codebase is in a working state after each slice, not mid-broken).
2. Identify the smallest slice that delivers real, visible value on its own — not necessarily the first thing listed, the smallest one that's actually useful by itself.
3. State which slice you're doing now and list every other slice as explicitly deferred, with a one-line reason it's not in scope right now.
4. Execute only the chosen slice.
5. When done, ask whether to continue to the next slice or stop here.

The task as given:
```

## Real example output
```
The task as given: "Clean up the auth system, it's a mess."

1. Slices identified:
   a. Consolidate 3 different "is logged in" checks (middleware, a hook, and a raw cookie read) into one source of truth.
   b. Remove the old session-based auth code path that's dead since the JWT migration 2 months ago.
   c. Add rate limiting to the login endpoint (currently unlimited attempts).
   d. Standardize error messages across login/signup/reset (currently inconsistent wording).

2. Smallest useful slice: (a), consolidating the auth checks. It's the one causing actual bugs right now (the hook and the middleware disagree on a stale cookie, which is why users sometimes see a flash of "logged out" on page load).

3. Doing (a) now. Deferred: (b) dead code removal — not urgent, no bugs caused by leaving it. (c) rate limiting — real gap but depends on you deciding whether to add a rate-limit library or roll a minimal one. (d) error message consistency — cosmetic, lowest value of the four.

4. Executed: unified into a single useAuth() hook backed by the JWT payload, removed the raw cookie read in Header.tsx and the redundant middleware check in 2 route files, all three now call useAuth().

5. Consolidation done, verified: the stale-cookie flash no longer reproduces after 10 manual reloads. Continue to the rate limiting decision, or stop here?
```

## Why it works
"Clean up X" requests carry hidden scope that balloons once you're inside the code. Forcing an explicit slice list before writing anything means the deferred work is a visible, intentional decision instead of work that silently never got finished, and the one slice that ships is small enough to actually review.
