---
title: "The Riskiest Assumption Kill Loop"
slug: "riskiest-assumption-kill-loop"
format: "loop"
category: "market-research"
tools: ["universal"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Rank a plan's load-bearing assumptions, then loop cheapest-test-first until each is validated or killed."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You have a business case, pitch draft, or launch plan that reads as convincing, and you know convincing documents hide their assumptions best. You want them dragged out, ranked, and tested one at a time until the plan is either de-risked or dead.

## The pattern
```text
Act as an assumption auditor. My plan is pasted below the line at the end
of this message. We're going to run a kill loop on it: you maintain a
ranked assumption ledger, I go gather evidence between rounds, and each
assumption ends as VALIDATED, KILLED, or BLOCKED. The plan only survives
if its load-bearing assumptions do.

ROUND 0, setup:

1. Extract every assumption the plan depends on, including the implicit
   ones the text never states (the buyer has budget authority, the channel
   still works at scale, the data is legally usable). Quote the line of my
   plan each assumption hides in.

2. For each, score:
   - BLAST RADIUS: if this is false, does the plan die (HIGH), limp
     (MEDIUM), or shrug (LOW)?
   - CURRENT EVIDENCE: NONE / ANECDOTE / DATA, based only on what my plan
     actually contains, not on how plausible the claim sounds
   Rank by HIGH blast radius + weakest evidence first. Plausibility is not
   evidence.

3. For the top-ranked assumption only: design the cheapest test that could
   falsify it within a week. Name the exact artifact I should bring back
   (call notes, a screenshot of pricing, a signup count), and state, before
   I go, what result kills the assumption and what validates it.

Each following round, I'll paste what I found. You then:

4. Rule on it: VALIDATED, KILLED, or STILL OPEN, judged against the kill
   condition you committed to last round, not against a new softer bar. If
   my evidence is thin or off-target, say STILL OPEN and repeat what's
   missing, don't grade on effort.

5. Reprint the full ledger with updated statuses, re-rank what's left, and
   issue the next test. If I can't run a test (no access, too slow), mark
   the assumption BLOCKED and visible, never quietly assume it true.

Stop conditions: any HIGH blast-radius assumption is KILLED (say plainly
the plan as written is dead and what a pivot would need to reuse), or all
HIGH assumptions are VALIDATED (output the surviving plan's remaining
MEDIUM risks as a watch list). Do not soften a kill into "needs more
research."

Paste your business case, pitch, or launch plan below this line:
```

## Real example output
```
Plan: sell an AI compliance-checklist tool to independent mortgage brokers

ROUND 0 LEDGER (top 4 of 9 extracted)
A1. Brokers do compliance themselves rather than outsourcing it
    ("brokers spend 6 hrs/wk on compliance" - plan, para 2)
    BLAST: HIGH / EVIDENCE: NONE
A2. Brokers will trust AI output for a liability-bearing task
    BLAST: HIGH / EVIDENCE: NONE (implicit, never stated in plan)
A3. $99/mo clears their software budget - BLAST: MEDIUM / EVIDENCE: ANECDOTE
A4. Facebook broker groups allow tool promotion - BLAST: LOW / EVIDENCE: NONE

TEST 1 (A1): call 5 brokers from the state license registry. Kill condition,
committed now: 3+ of 5 say their compliance is handled by their brokerage
or an outsourced service. Validation: 3+ do it personally weekly.

[round 2, user pastes call notes from 6 brokers]

RULING A1: KILLED. 5 of 6 said the brokerage's back office handles file
compliance; only the one solo broker matched the plan's persona. That
exceeds the committed kill condition (3+ of 5).

LEDGER: A1 KILLED, A2-A4 now moot in current form.

STOP: A1 was load-bearing, the plan as written is dead. Salvageable pivot:
the back offices doing the work at 40-file volume are a real buyer with
the same pain, one A1-style validation round on back-office managers would
tell you if the pivot inherits the plan or the graveyard.
```

## Why it works
Committing the kill condition before evidence comes back is what makes the ruling honest; without it, every ambiguous result gets read as survival. Ranking by blast radius times evidence gap, instead of by what's easiest to check, forces the uncomfortable tests first, which is where one week of calls can save a quarter of building.
