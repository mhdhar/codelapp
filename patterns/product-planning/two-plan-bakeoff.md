---
title: "Two Plans Enter, One Plan Ships"
slug: "two-plan-bakeoff"
format: "prompt"
category: "product-planning"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Force two rival plans, cheapest and most thorough, then make the agent attack both before you pick one."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to ask an agent to plan a feature and you know from experience it will produce one plan and then defend it. Use this when the real question isn't "how do we build it" but "how much of it deserves building."

## The pattern
```text
Before implementing the task I describe below the line, give me two rival plans.

PLAN A, the cheapest version: the smallest change that plausibly solves it. Files touched, steps, and what it deliberately ignores.
PLAN B, the thorough version: what you'd build with a free week. Files touched, steps, and what it adds beyond A.

Then attack both. Read the codebase first if you need to:
- The strongest realistic case that A bites us within a month, tied to something concrete in this repo, not a hypothetical.
- The strongest realistic case that B is wasted effort, equally concrete.

Finish with: which plan you'd pick today, and the single observable condition that would flip the choice (a number, an event, a support pattern). Then stop and wait for my pick. Don't start building.

My task:
```

## Real example output
"PLAN A: dedupe uploads with a filename-plus-size check in api/uploads.ts. 1 file, ~20 lines. Ignores content-identical files uploaded under different names.

PLAN B: content hashing with a file_hashes table, a backfill job, and a hash check on upload. 4 files plus a migration.

AGAINST A: the mobile app renames every export before upload (mobile/src/export.ts appends a timestamp), so identical content with different names is the common case here, not the edge case.

AGAINST B: the backfill has to hash 40 GB of existing uploads and this repo has no job queue to run it on. That's new infrastructure for a dedupe feature.

PICK: A today. Flip to B the first time renamed-duplicate uploads appear in more than one support ticket in a week. Waiting for your pick."

## Why it works
One plan invites anchoring, the model commits to whatever it drafted first and argues for it. Two plans plus a forced attack on each makes the real tradeoff visible, and the flip condition converts "we'll revisit this later" into a test you can actually observe.
