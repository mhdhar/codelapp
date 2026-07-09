---
title: "Cull the Backlog Against the North-Star Goals"
slug: "backlog-cull-against-north-star"
format: "workflow"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Run the backlog through the spec's original goals and cut what doesn't serve any of them."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The backlog has grown past what the original spec scoped, a few sprints in, and nobody has checked whether the newer items still serve the goals that justified the project. Run this before planning the next sprint, not after it's already overcommitted.

## The pattern
Step 1, classify:
```text
Cull this project's backlog against its original goals.

First, find the goals this project was scoped against: check the repo for a spec or PRD (docs/, README, anything named spec or prd). If none exists, stop and ask me for the goals before going further.

Then, for each backlog item, classify:
- SERVES: which specific goal it directly advances (name it, don't say "improves the product")
- ORPHAN: doesn't map to any goal, exists for another reason (say what reason, e.g., "someone asked for it," "seemed cool")
- CONFLICTS: actively works against a goal (e.g., adds complexity to something the goals say to keep simple)

Recommend: keep, cut, or defer for each ORPHAN and CONFLICT item. Don't touch SERVES items.

Find the backlog the same way: check the repo for a tracker file (TODO.md, BACKLOG.md, an issues export). If there isn't one, ask me to paste the current backlog and wait for it before classifying anything.
```

Step 2, protect sunk work:
```text
Of the items marked cut or defer, are any of them already in progress or promised to someone? List those separately, since cutting them has a cost beyond backlog hygiene, and needs a human call, not an automatic cut.
```

## Real example output
"SERVES (goal: reduce checkout abandonment by 15%): saved-card pre-selection, inline card switcher, guest-checkout autofill.
ORPHAN: 'add dark mode to checkout' — reason given was 'a user asked for it once.' No goal maps to it. Recommend: defer, revisit if it shows up in more than one support ticket.
CONFLICT: 'add 4 new payment provider options' — goals explicitly list a non-goal of 'not building new payment providers.' Recommend: cut, it directly violates a stated non-goal.

IN PROGRESS / PROMISED: dark mode is already half-built by a contractor and was verbally promised for this release — flagging for your call, not auto-deferring."

## Why it works
Naming SERVES/ORPHAN/CONFLICT forces every backlog item to justify itself against a goal that was written down before feature requests started arriving, instead of against whoever asked most recently or loudest. Separating in-progress items from the cut list keeps the exercise from silently killing sunk work without a human decision.
