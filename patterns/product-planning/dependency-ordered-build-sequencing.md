---
title: "Order Features by Dependency Before Sequencing the Roadmap"
slug: "dependency-ordered-build-sequencing"
format: "workflow"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Turn a flat feature list into a dependency-ordered build sequence before assigning sprints."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You have an approved feature list or backlog and you're about to assign it to sprints or tickets in the order it was written down — which is usually the order people thought of it in, not the order it can actually be built in.

## The pattern
Step 1, map dependencies:
```text
Map the dependencies in the feature list I give you.

For each feature, identify:
1. What it depends on (other features on this list, or "none")
2. What depends on it (downstream features that can't start until this ships, or "none")
3. Whether it's a leaf (no dependencies, buildable immediately), composite (depends on other features here), or integration (touches multiple other features/systems)

Then output a build order: leaves first, grouped by what unlocks the most downstream work, integration features last. Flag any circular dependency you find, that's a scoping bug, not something to sequence around.

Start by asking me for the feature list: either paste it or name the file in this repo that holds it. Wait for my reply before mapping anything.
```

Step 2, once you've reviewed the order:
```text
Approved. Convert the build order into numbered phases, each phase only containing features with no unresolved dependencies from earlier phases. Note which phases can run in parallel across different people/agents versus which must be sequential.
```

## Real example output
"LEAVES: user auth (none), receipt upload (none), category taxonomy (none).
COMPOSITE: expense categorization (depends on: receipt upload, category taxonomy), monthly report (depends on: expense categorization).
INTEGRATION: partner API export (depends on: expense categorization, auth — needs API-key scoping across both).

No circular dependencies found.

PHASE 1 (parallel): user auth, receipt upload, category taxonomy.
PHASE 2 (sequential, needs Phase 1 complete): expense categorization.
PHASE 3 (parallel): monthly report, partner API export — both only need categorization, not each other."

## Why it works
Naming leaf/composite/integration forces an explicit dependency graph instead of an implicit one, so "build this first" decisions come from what's actually blocking what, not from the order items appear in a meeting note. The circular-dependency flag catches a real scoping bug before it becomes a stuck sprint.
