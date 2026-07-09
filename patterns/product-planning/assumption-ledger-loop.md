---
title: "Keep a Running Assumption Ledger Through Planning"
slug: "assumption-ledger-loop"
format: "loop"
category: "product-planning"
tools: ["universal"]
difficulty: "advanced"
est_time: "5 min per session, ongoing"
models: ["any"]
summary: "Log every silent planning assumption in a ledger and force confirmation before you build on it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Planning spans multiple sessions (a spec one day, tickets the next, architecture the day after), and each session quietly assumes things the last one didn't write down. Run this across the whole planning arc to catch assumptions before they calcify into decisions nobody actually made.

## The pattern
```text
For all planning work on this project from now on, keep an assumption ledger.

Every time you make a judgment call I didn't explicitly specify (a default value, a "probably means," an inferred priority, a guessed constraint), add a row:
- Assumption: what you assumed
- Why: what forced the guess (nothing in the spec covered it, conflicting signals, etc.)
- Risk if wrong: LOW / MEDIUM / HIGH, one line on the blast radius
- Status: UNCONFIRMED

At the start of each new planning session, show me the full ledger sorted by risk (HIGH first) before doing new work. I'll mark each row CONFIRMED or FLIPPED. Anything still UNCONFIRMED after two sessions gets flagged as blocking, don't build on top of a HIGH-risk unconfirmed assumption, ask instead.
```

## Real example output
"LEDGER (session 3):
1. Assumption: 'monthly report' means calendar month, not billing cycle. Why: spec says 'monthly' without defining the boundary. Risk: MEDIUM (affects report date-range logic in 2 files). Status: UNCONFIRMED (2nd session flagged).
2. Assumption: bulk CSV upload caps at 500 rows. Why: no limit specified, picked a default. Risk: LOW (easy to change later, not architectural). Status: CONFIRMED (approved in session 2).

BLOCKING: item 1 is UNCONFIRMED after 2 sessions and MEDIUM risk, touching report logic already partially built. Stopping report date-range work until you confirm calendar month vs. billing cycle."

## Why it works
Most planning drift isn't a wrong decision, it's an unmade decision that got treated as settled because nobody wrote it down. A ledger that resurfaces high-risk unconfirmed items at the start of every session turns "I assumed X" from a postmortem line into something caught while it's still cheap to fix.
