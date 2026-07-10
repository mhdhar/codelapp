---
title: "Competitor Positioning Matrix, One Rival at a Time"
slug: "competitor-positioning-matrix-loop"
format: "loop"
category: "market-research"
tools: ["universal"]
difficulty: "intermediate"
est_time: "25 min"
models: ["any"]
summary: "Feed competitor pages in one at a time and build a running positioning matrix that ends in a whitespace claim."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["positioning-gap-analysis", "competitor-messaging-audit", "category-creation-vs-incumbent-framing"]
---

## When to use this
You're positioning a new product against 4-8 competitors and keep losing track of who claims what. You want one matrix built from their actual public copy, not from your memory of their homepages.

## The pattern
```text
Act as a competitive positioning analyst. I'm going to paste competitor
material one competitor at a time: homepage copy, pricing page text,
tagline, whatever I can grab. You maintain ONE running positioning matrix
across all rounds. It gets sharper each round, not longer.

Each round, do the following:

1. From the pasted material only, extract for this competitor:
   - CLAIMED AUDIENCE: who their copy says they're for, quoted where possible
   - CLAIMED WEDGE: the one thing they lead with (speed, price, compliance,
     integrations, "all-in-one"). Take it from headline and first section,
     not buried feature lists.
   - VALUE METRIC: how they charge (per seat, usage, flat), if the material
     shows it. If it doesn't, write "not shown," don't guess.
   - PROOF STYLE: what they use to back the claim (logos, numbers, reviews,
     nothing)

2. Add a row to the matrix and reprint the FULL matrix, all competitors so
   far, so I always see the whole board.

3. CROWDING CHECK: after each row, say which wedge is getting crowded (2+
   competitors leading with the same claim) and which claimed audiences
   overlap.

4. Quote only text that is actually in what I pasted. If a cell can't be
   filled from the material, mark it "not shown" instead of inferring from
   what you remember about this company. Your memory of their marketing may
   be stale, the pasted copy is the source of truth.

After each round I'll paste the next competitor. When I say "map it":

5. Output the final matrix, then name the whitespace: audience/wedge
   combinations no one in the matrix claims. For each, say whether it's
   empty because it's genuinely open or because it's probably a bad
   business, don't treat every gap as an opportunity.

6. End with the single positioning line you'd test for my product and which
   competitor's customers it would pull from first, using the one-line
   product description I gave you at the start.

Start by asking me in one message for: (1) my product in one line, what it
is and for whom, (2) the first competitor's homepage/pricing copy. Wait for
my reply before building the first row.
```

## Real example output
```
Product: error-tracking tool for mobile-first startups

ROUND 4 (after pasting Sentry, Bugsnag, Crashlytics, Rollbar copy)

| Competitor  | Claimed audience        | Claimed wedge            | Value metric | Proof style        |
|-------------|-------------------------|--------------------------|--------------|--------------------|
| Sentry      | "every developer"       | breadth, "code breaks... | per event    | logos + OSS badge  |
| Bugsnag     | "engineering teams"     | stability scores/SLAs    | per event    | customer numbers   |
| Crashlytics | mobile app developers   | free, Firebase-native    | free         | Google brand       |
| Rollbar     | "developers"            | AI-assisted grouping     | per event    | logos              |

CROWDING CHECK: "for developers, per-event pricing" is claimed by 3 of 4.
Mobile-specific is claimed only by Crashlytics, and only as a free bundled
feature, not a paid standalone.

[user says "map it"]

WHITESPACE: paid, mobile-first, per-app flat pricing is unclaimed. Likely
genuinely open rather than a dead zone: Crashlytics proves mobile demand
exists but its free tier caps how seriously Google serves power users
(no custom retention, weak alerting per the pasted copy).

POSITIONING LINE TO TEST: "Error tracking built for mobile release cycles,
flat price per app, not per crash." Pulls first from Crashlytics users who
outgrew free, not from Sentry loyalists.
```

## Why it works
Feeding one competitor per round keeps each extraction grounded in that competitor's actual copy instead of the model's stale memory of a category. Reprinting the full matrix every round means crowding and whitespace emerge from accumulated evidence, and the "map it" gate stops the model from calling whitespace before the board is full.
