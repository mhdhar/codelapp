---
title: "Turn an Approved Spec into Engineering-Ready Tickets"
slug: "spec-to-engineering-tickets"
format: "workflow"
category: "product-planning"
tools: ["universal"]
difficulty: "beginner"
est_time: "15 min"
models: ["any"]
summary: "Split a locked spec into sized tickets with acceptance criteria instead of one vague epic."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The spec is approved and you're about to hand it to an agent or a teammate to build, but "implement the spec" is still one giant task with no clear stopping points.

## The pattern
Step 1, break it down:
```text
Here's an approved spec: [PASTE SPEC].

Break it into tickets. For each ticket:
1. Title, one line, verb-first (e.g., "Add rate limit to login endpoint")
2. Acceptance criteria: a checklist of testable conditions
3. Size: S (under 2 hrs), M (half day), L (full day+) — if anything is L, say why it can't be split further
4. Depends on: which other tickets, if any (by title)

Flag any P0 requirement in the spec that doesn't map cleanly to a ticket, that's a gap in the spec, not something to silently interpret.
```

Step 2, once you've reviewed the tickets:
```text
Approved. Order the tickets into a build sequence respecting the dependencies you listed, and group them into a first milestone (P0 only) and a second milestone (P1+).
```

## Real example output
"TICKET 1: Add saved-card pre-selection at checkout. AC: returning customer sees last-used card pre-selected, single tap completes payment, falls back to card picker if no saved card. Size: M. Depends on: none.
TICKET 2: Add inline card switcher. AC: user can swap cards without leaving checkout page, selection persists for session. Size: S. Depends on: Ticket 1.
GAP FLAGGED: the spec's P0 'reduce support tickets by half' has no corresponding ticket — it's an outcome metric, not a buildable requirement. Confirm what change actually drives it (likely clearer error messaging on card failure) before this milestone is called complete.

MILESTONE 1 (P0): Tickets 1-2.
MILESTONE 2 (P1): card nicknames, expiry warnings."

## Why it works
Sizing and acceptance criteria per ticket give whoever picks it up a concrete stopping point instead of one open-ended epic. The gap-flagging step catches the common case where a spec's goal (a metric) gets treated as a requirement (a feature) and silently dropped.
