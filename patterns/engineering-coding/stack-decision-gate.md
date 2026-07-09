---
title: "Stack Decision Gate"
slug: "stack-decision-gate"
format: "goal"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Force an explicit preset-vs-custom decision before scaffolding a new project."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to start a new project or a large new subsystem and haven't consciously decided whether to reach for an existing full-featured starter/preset or build the architecture by hand. Use it before writing any scaffolding code.

## The pattern
```text
I'm starting a new project/subsystem, described at the end of this message. Help me pass through a stack decision gate before any code gets written.

1. Check whether my must-have features match what a known full-featured preset/starter already provides out of the box (name specific real options, not hypothetical ones).
2. If a preset covers 70%+ of the must-haves: recommend it by name, list exactly which must-haves it covers vs. which I'd still build myself, and give the actual init command.
3. If no preset covers enough, or my constraints rule presets out (e.g. must integrate with an unusual existing system), recommend a custom architecture instead — but justify why, don't default to custom out of habit.
4. Either way, state the one thing that would flip your recommendation.
5. Give me the concrete next command to run.

What I'm building:
Must-have features:
Constraints (deadline, team size, existing infra):
```

## Real example output
```
What I'm building: internal tool for the ops team to track vendor contracts and renewal dates.
Must-have features: email/password login, a table view with filters, CSV export, deadline reminder emails.
Constraints: 2-week deadline, solo dev, must run on our existing Vercel + Postgres setup.

1. This doesn't need payments, multi-tenancy, or a marketing site — the must-haves are auth + CRUD + a scheduled email job. Full SaaS starters cover far more than needed (Stripe, landing page, admin analytics) and would add setup overhead you'd immediately rip out.

2. Preset coverage: not worth it here — you'd spend more time deleting unused billing wiring than you'd save.

3. Recommend custom, minimal architecture: Next.js App Router + your existing Postgres, Auth.js for email/password (skip building auth from scratch, that part IS worth reaching for a library), a Vercel Cron job for the reminder email check.

4. What would flip this: if ops later wants self-serve billing per vendor or multi-team access, revisit — that's when a fuller SaaS preset starts paying for itself.

5. Next command: `npx create-next-app@latest vendor-tracker --typescript --app`, then `npm i next-auth` for step one.
```

## Why it works
The instinct to reach for the biggest available starter, or the opposite instinct to build everything by hand out of habit, both cost real time. Naming the actual must-haves and checking them against what a preset covers turns a vibe-based choice into an evidence-based one.
