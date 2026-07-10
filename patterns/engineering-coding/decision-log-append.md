---
title: "Decision Log Append"
slug: "decision-log-append"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Log every real technical decision so it never gets silently relitigated by a future session."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["decision-batch-checkpoint", "project-state-handoff-file", "assumption-ledger-loop"]
---

## When to use this
A project spans many sessions or multiple people/agents, and you keep noticing the same architectural question getting re-debated because nobody remembers it was already settled, and why. Use this every time a real technical decision gets made, not for routine changes.

## The pattern
```text
Whenever you make a decision in this session that a future session might reasonably second-guess or redo, log it.

A decision qualifies if: it picked between two or more real alternatives, it would take more than a sentence to re-explain, or reversing it later would cost real rework.

For each qualifying decision, append an entry to DECISIONS.md at the repo root (create the file if it doesn't exist; if the repo already has a decisions log under another name, use that one). Each entry follows this shape:

## <today's date> - <one-line decision title>
Chose: <what was chosen>
Over: <what was considered and rejected>
Why: <the actual reason, tied to this project's constraints, not a generic best practice>
Reversible: <one of: yes, easily / yes, with rework / no, load-bearing>

At the end of the session, list every entry you added - don't just silently append, tell me what got logged.
```

## Real example output
```
DECISIONS.md, 2 entries added this session:

## 2026-07-09 - Session storage: JWT in httpOnly cookie, not localStorage
Chose: JWT stored in an httpOnly cookie.
Over: localStorage token (simpler client code, considered first since the mobile team suggested it for parity).
Why: this project doesn't have a mobile app yet (future roadmap item, not current scope), and httpOnly avoids XSS token theft, which matters more here since we render some user-submitted markdown.
Reversible: yes, with rework - would need a token refresh strategy change if a mobile client gets added later.

## 2026-07-09 - Background jobs: Postgres-backed queue, not Redis
Chose: a simple jobs table in the existing Postgres DB, polled every 10s.
Over: Redis + BullMQ (more standard for job queues, considered because it's what I've used before).
Why: this app already runs Postgres and has near-zero job volume (under 50/day estimated) - adding Redis would be a new service to provision, monitor, and pay for, to solve a scale problem this project doesn't have.
Reversible: yes, easily - job table schema is generic enough to migrate to Redis later if volume grows past what polling can handle.

Logged both. No other decisions this session met the bar (the pagination limit of 25 was a default, not a real fork in the road).
```

## Why it works
Without a written record, every new session either re-derives the same reasoning from scratch or, worse, silently reverses a decision that was already made for a good reason. Logging only decisions that actually forked keeps the file worth reading instead of becoming noise nobody opens.
