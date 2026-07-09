---
title: "Re-Check the Complexity Tier When Scope Moves Mid-Build"
slug: "complexity-drift-recheck-loop"
format: "loop"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min per check, ongoing"
models: ["any"]
summary: "Force a tier re-classification every time a new requirement lands mid-build, catching drift early."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You gated a task at a fixed complexity tier before starting, but you're several requirements deep into the build and nobody has checked whether it's quietly grown past that tier. This is the loop that runs during execution, not the one-time gate before it — pair it with an upfront complexity gate, this one keeps checking after that gate is behind you.

## The pattern
```text
We classified this task's complexity earlier in this session. Restate that classification now: the tier (TRIVIAL / MODERATE / COMPLEX) and the file/scope list we agreed on. If we never ran one, classify it now and show me before continuing.

From now on, every time a new requirement, edge case, or "while we're at it" request comes up during this build, before touching it:

1. State the new requirement in one line.
2. Re-run the tier classification (TRIVIAL / MODERATE / COMPLEX) for the task AS IT NOW STANDS, including this new piece, not just the new piece alone.
3. If the tier is unchanged, proceed.
4. If the tier increased, stop before implementing the new requirement. Report: original tier, new tier, what specifically pushed it up, and the updated file/scope list. Wait for my go-ahead before continuing.

Do this check for every new requirement, no exceptions, even ones that sound small ("just add one more field").
```

## Real example output
"NEW REQUIREMENT: 'also let admins bulk-edit refund status from a CSV upload.'

RE-CLASSIFICATION: Original tier was MODERATE (refund_status column, 2 files). With CSV bulk upload added: now COMPLEX — introduces file upload handling, a new background job for bulk processing, and a new admin-only permission check that didn't exist before.

TIER INCREASED. Stopping before implementing. Updated scope would add: an admin bulk-refunds endpoint, a queue worker, and a new admin role check in the auth middleware. Waiting for approval before continuing — this is materially bigger than what was originally scoped."

## Why it works
Scope creep rarely arrives as one big change, it arrives as a string of small "while we're at it" additions that each look trivial alone. Re-running the same fixed tier classification on the cumulative scope, not just the newest piece, is what catches the drift that a one-time gate at the start can't.
