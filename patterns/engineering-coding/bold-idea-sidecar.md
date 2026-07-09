---
title: "Bold Idea Sidecar"
slug: "bold-idea-sidecar"
format: "prompt"
category: "engineering-coding"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Get one honest, out-of-the-box improvement idea alongside routine execution, without derailing the task."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You want an agent to do the assigned task exactly as scoped, but you also want it thinking one level up instead of purely executing. Use it as a standing instruction on top of any task, not as a replacement for proper scoping.

## The pattern
```text
Do the task I give you exactly as scoped. Do not expand it.

After you finish and report the result, add one more section titled "Bold idea" — but only if you have a genuinely good one, not by default. A Bold idea must be:
- Realistic to build (not "rewrite this in Rust")
- Something you noticed specifically because of doing this task, not a generic suggestion
- Explained in 2-3 sentences: what it is, why it would help, and roughly how big a lift it is

If you don't have a real one, write "No bold idea this time" instead of forcing something mediocre. Never let the Bold idea block or expand the actual task.

First ask me for the task, then wait. Don't start until you have it.
```

## Real example output
```
> The task: Add a "resend verification email" button to the account settings page.

Done. Button added to /settings, calls POST /api/auth/resend-verification, disabled for 60s after click to prevent spam-clicking. Verified by triggering it twice in a row — second click stayed disabled until the cooldown expired.

Bold idea: While wiring this up I noticed verification emails go out through a single synchronous SendGrid call inside the request handler — if SendGrid is slow, the whole request blocks on it. Moving email sends to a queue (even a simple one, like a Postgres-backed job table) would make signup latency independent of email provider latency. Medium lift, maybe half a day, and it'd fix a latency spike I saw in the logs (SendGrid call took 2.8s during today's test).
```

## Why it works
Separating "do the task" from "here's something bigger I noticed" keeps execution disciplined while still capturing the insight an agent gets from being inside the code, something a human skimming a ticket wouldn't see. Making it optional stops it from turning into padding.
