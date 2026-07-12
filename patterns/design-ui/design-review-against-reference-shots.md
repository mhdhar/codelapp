---
title: "Hit a Named Quality Bar: Design Review Against Reference Screenshots"
slug: "design-review-against-reference-shots"
format: "goal"
category: "design-ui"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Compare your UI with a reference screenshot, name material gaps, and verify any approved changes."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["ai-slop-critique-loop", "rubric-grading-loop", "ui-approval-gate-checkpoints"]
---

## When to use this
You know your UI is "worse" than some reference - a competitor, an inspiration site, an earlier version of your own product - but can't articulate why. Use this when you have real screenshots of both, not just a vibe, and want the specific gap closed rather than a random restyle.

## The pattern
```text
GOAL: Bring my screen up to the visual quality of the reference screenshot, closing the
specific, nameable gaps between them - not a random restyle.

Inputs: two screenshots. A is my current UI; B is the reference. Both show a similar
type of screen/component. Find the component(s) in this codebase that render
screenshot A before you change anything.

SUCCESS CRITERIA:
1. You've named up to 5 material, specific differences between A and B - not vague
   ("B feels more premium") but specific ("B uses 3 font weights across the hierarchy, A
   uses 1"; "B has 40px of breathing room around the primary action, A has 12px").
2. For each named difference, you've decided: adopt it, adapt it to fit our existing brand,
   or explicitly reject it with a one-line reason (e.g. "B's dark background doesn't fit
   our light-only brand, skip").
3. Before changing code, present the adopt/adapt decisions and wait for my approval. If I approve implementation, make only those approved changes.
4. You've taken a new screenshot of the result and can point to each of the 5+ differences
   and state, with evidence, whether it's now closed, partially closed, or intentionally
   not closed (with the reason from step 2).
5. The result still passes our existing design constraints (uses only our DESIGN_SYSTEM.md
   tokens if we have one, doesn't break any existing responsive breakpoint).

You choose the order and how many iterations it takes. Report the comparison and proposed decisions first. After approved implementation, report the final screenshot and the closed/rejected list; explicitly list anything not verified.

Start by asking me to attach both screenshots (A, then B) in one message, then wait
until you have them before doing anything else.
```

## Real example output
```
Compared: our /patterns category page (A) vs. Linear's changelog page (B, reference for
information density done well).

NAMED DIFFERENCES:
1. B groups items by date with a sticky section header; A has one flat list with no grouping.
   -> ADOPT: group patterns by category with a sticky header.
2. B uses a 2px left border in brand color to mark the currently-active nav item; A has no
   active-state indicator in the sidebar.
   -> ADOPT: added active border using our existing --accent token.
3. B's body text is 14px with 1.6 line-height; A's is 16px with 1.4 - B reads denser but
   more scannable for a list of many short items.
   -> ADAPT: dropped to 15px/1.5, kept 16px on titles - full 14px felt too small against
   our existing type scale.
4. B has no card borders, just spacing and a hover background; A puts a border around
   every card.
   -> ADOPT: removed card borders, added subtle hover background (neutral-100).
5. B's dark theme uses a near-black background (#0A0A0A); A's dark theme uses a lighter
   dark gray (#1A1A1A).
   -> REJECT: our dark theme token is already used across 20 other patterns, changing it
   here alone would create inconsistency. Not a fix for this task.

IMPLEMENTED: 1, 2, 3, 4 in PatternGrid.tsx and PatternCard.tsx.
VERIFIED: new screenshot taken, compared side by side. 1, 2, 4 fully closed. 3 partially
closed (deliberately, per adaptation reasoning). 5 explicitly rejected with reason recorded.
All existing DESIGN_SYSTEM.md tokens still respected, checked at 375/768/1024/1920px.
```

## Why it works
"Make it look better" gives the model nothing to converge on, but a side-by-side reference with named, itemized differences turns a vague quality gap into a checklist it can actually close and you can actually verify. Forcing an explicit reject-with-reason stops it from copying every surface trait of the reference, including ones that don't fit your brand.
