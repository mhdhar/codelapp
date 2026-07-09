---
title: "Pre-Mortem the Plan Until It Stops Dying"
slug: "premortem-until-it-stops-dying"
format: "loop"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "25 min"
models: ["any"]
summary: "Assume the project failed, name the killers, patch the plan, repeat until a fresh pre-mortem finds nothing new."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The plan or spec is written and everyone likes it, which is exactly when nobody is looking for the thing that kills it. Run this once before the plan is locked, after the goals and scope are on paper but before the first ticket is cut.

## The pattern
```text
Run a pre-mortem loop on this project's plan. Find it first: use the plan or spec file in this repo if one exists (docs/, anything named plan, spec, or prd) and confirm the file with me. If there isn't one, ask me to paste the plan and wait for it before round 1.

Each round:
1. Assume it's six months later and this project failed badly. Write the 5 most plausible causes of death. Each cause must point at something specific in THIS plan (a named feature, dependency, estimate, or assumption), not a generic risk like "ran out of time."
2. Rate each cause LIKELY, POSSIBLE, or STRETCH.
3. For every LIKELY cause, patch the plan with a concrete change: cut scope, add a checkpoint, reorder work, or add a fallback. Show each patch as old line, then new line.
4. Re-run the pre-mortem against the patched plan. Causes already patched don't count as new.

Exit condition: one full round finds zero new LIKELY causes. STRETCH causes never block exit, collect them in a final "accepted risks" list instead.
Hard cap: 4 rounds. If you hit the cap and LIKELY causes keep appearing, stop and say the plan is overcommitted, not patchable, and name the piece you'd cut first.
```

## Real example output
"ROUND 1: LIKELY: 'Marketplace payouts estimated at 2 days, but Stripe Connect onboarding needs identity verification, typically 1-2 weeks per seller cohort.' Patch: old 'Milestone 1: payouts live', new 'Milestone 1: manual payouts via admin, Connect moves to Milestone 2.'

ROUND 2: LIKELY: 'Manual payouts assume an admin queue that appears nowhere in the ticket list.' Patch: added 'Admin payout queue (size M)' to Milestone 1.

ROUND 3: zero new LIKELY causes. Exiting after 3 rounds.

ACCEPTED RISKS (STRETCH): App Store review rejects the external checkout link. Seller cohort 1 is smaller than the 50 assumed in the revenue goal."

## Why it works
Asking "what could go wrong" gets polite answers, asking "why did this die" gets specific ones, because the model has to work backward from a failure instead of defending the plan. The loop matters as much as the question: each patch gets re-attacked in the next round, so fixes that quietly create new failure modes get caught before the plan is locked.
