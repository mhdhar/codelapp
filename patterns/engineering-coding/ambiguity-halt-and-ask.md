---
title: "Ambiguity Halt and Ask"
slug: "ambiguity-halt-and-ask"
format: "prompt"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Make the agent name exactly what's unclear and stop, instead of guessing silently and building on it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're handing off a task where the spec has gaps — a new feature, an API contract, a data model — and a wrong silent guess would be expensive to unwind. Use it before implementation starts, not as damage control after.

## The pattern
```text
Before writing any code for the task I give you, check it for ambiguity. Do not guess and proceed silently.

1. Read the task and any related existing code.
2. List every point that's genuinely ambiguous — split into:
   - BLOCKING: you cannot make a reasonable default choice here; getting it wrong means rework. Ask me directly.
   - NON-BLOCKING: you could reasonably pick a default, but state the default you'd pick and why, so I can correct it in one line if it's wrong.
3. If there are zero BLOCKING items, say so and proceed with the NON-BLOCKING defaults stated.
4. If there is at least one BLOCKING item, stop entirely. Do not write code, do not start on the unambiguous parts first. Ask your BLOCKING questions as a numbered list and wait.

First ask me for the task, then wait. Don't start until you have it.
```

## Real example output
```
> The task: "Add a way for admins to bulk-deactivate users."

BLOCKING:
1. "Deactivate" isn't defined anywhere in the codebase — is this a soft flag (is_active=false, user stays in DB, reversible) or does it also need to revoke active sessions/tokens immediately? These are very different implementations.
2. "Bulk" — is there an upper limit? The users table has 40k rows; a bulk action with no cap could be a UI someone fat-fingers into deactivating everyone.

NON-BLOCKING (proceeding with these unless you correct me):
- Assuming this is admin-only, gated by the existing requireAdmin middleware already used on /admin/* routes.
- Assuming deactivated users see a generic "account deactivated, contact support" message on next login, matching the pattern used for banned users in auth/middleware.ts.

Stopping here — need answers to the 2 BLOCKING questions before writing anything.
> 1. Soft flag only, no session revocation for now. 2. Cap it at 500 per action.

Proceeding with soft-deactivate, 500-row cap, admin-gated, generic message on login.
```

## Why it works
Splitting blocking from non-blocking stops two failure modes at once: the agent that silently guesses on something expensive, and the agent that stalls the whole task over a detail you'd have been fine either way on. You only get interrupted for the guesses that actually matter.
