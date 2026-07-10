---
title: "Complexity-Gated Scoping Before Any Build"
slug: "complexity-gated-scoping"
format: "workflow"
category: "product-planning"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Classify a task's complexity first, then gate execution behind your approval so agents stop over- or under-building."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["complexity-tier-classifier", "complexity-drift-recheck-loop", "smallest-useful-move-scoper"]
---

## When to use this
You're about to hand an agent a task and you don't yet know if it's a 10-minute tweak or a multi-file redesign. Skip this and you either get a five-line fix for something that needed a migration plan, or a full rewrite for something that needed a one-liner.

## The pattern
```text
Before writing any code, classify the complexity of the task I give you.

Pick one tier and justify it in 2-3 sentences:
- TRIVIAL: single file, no new dependencies, no schema or API changes.
- MODERATE: multiple files or one new dependency, no breaking changes to existing contracts.
- COMPLEX: schema changes, new APIs, cross-cutting changes, or anything that breaks an existing contract.

Then list exactly what you'd touch (files, schemas, APIs) for this tier. Do not write any implementation code yet. Stop and wait for my go-ahead.

Once I approve, proceed with the plan you outlined, at the tier you classified. Stick to the files and scope you listed; if you discover you need to touch something outside that list, stop and tell me why before continuing.

Start by asking me to describe the task, then wait for my reply before classifying anything.
```

## Real example output
"CLASSIFICATION: MODERATE. This touches the `orders` table (new `refund_status` column) and two files (`api/orders.ts`, `components/OrderCard.tsx`). No breaking changes to existing endpoints since the column is additive and nullable.

Files to touch: `db/migrations/`, `api/orders.ts`, `components/OrderCard.tsx`.

Waiting for approval before implementing."

## Why it works
Naming three fixed tiers gives the model a forced choice instead of open-ended judgment, which is what keeps a "fix this typo" request from turning into a refactor. The explicit stop-and-wait step is the actual gate: it moves the scope decision to you before any code exists, so rework happens on a plan, not a diff.
