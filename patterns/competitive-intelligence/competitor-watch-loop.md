---
title: "Competitor Watch Loop"
slug: "competitor-watch-loop"
format: "loop"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "A weekly check that flags only what changed on a competitor's public pages, not the whole page again."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You want to track a competitor over time without re-reading their entire site every week. Run this once, save the output, then run it again next week and diff it yourself, or feed last week's snapshot back in as context.

## The pattern
```text
Visit [COMPETITOR_URL] (and its /pricing and /changelog pages if they exist). This is a recurring weekly check, not a first-time scan.

Previous snapshot from last check:
[PREVIOUS_SNAPSHOT]

Compare the current state of the pages against that snapshot. Report only:
1. Anything new: new pricing tier, new feature, new page, new messaging
2. Anything changed: price moved, a feature description changed, a plan was renamed or removed
3. Anything removed: a feature, tier, or page that was there before and isn't now

If nothing changed in a category, write "No change" for it, don't restate the old content. End with an updated snapshot block I can paste in next time, covering current pricing tiers and feature list in short bullet form.
```

## Real example output
New: Added a "Teams" tier at $49/mo between Starter and Growth.
Changed: Growth tier price moved from $99/mo to $89/mo (annual discount now shown by default).
Removed: No change.

Updated snapshot:
- Starter: $29/mo, 3 seats, 10k API calls
- Teams: $49/mo, 8 seats, 40k API calls (new)
- Growth: $89/mo, 15 seats, 100k API calls
- Enterprise: custom, unlimited seats, SSO

## Why it works
Feeding last week's snapshot back in turns a full re-read into a diff. The model only has to report deltas, which keeps output short enough to actually skim weekly instead of skipping it. The snapshot block at the end is what makes the loop self-sustaining, no separate tracking system needed.
