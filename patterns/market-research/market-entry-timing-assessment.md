---
title: "Market-Entry Timing Assessment: Why Now"
slug: "market-entry-timing-assessment"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Stress-test whether now is the right time to enter a market, or if you're early, late, or on time."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You have a product idea that could plausibly work and need to separate "this is a good idea" from "this is a good idea right now," before you commit real budget or engineering time to it.

## The pattern
```text
Act as a market-timing analyst.

Do the following:

1. ENABLING SHIFTS: name the specific technology, cost, regulatory, or
   behavioral shifts in the last 12-24 months that make this newly possible
   or newly needed. Shifts from 5+ years ago don't count, those would
   already have been acted on. Tag each as SOURCED (name the specific
   event/data point) or ESTIMATE (your inference).

2. TOO-EARLY SIGNALS: name what would tell you this market isn't ready yet
   (enabling infrastructure not built, buyer lacks budget authority, the
   workaround pain isn't acute enough). Say whether each is currently
   present or absent, based on what you actually know.

3. TOO-LATE SIGNALS: name what would tell you incumbents or well-funded
   competitors already have this locked (a dominant player already raised
   a large round for this exact thing, the market is already consolidating).

4. CURRENT READ: given 1-3, state plainly whether this looks too early, too
   late, or right on time. Name what you're most uncertain about in that
   call. Don't force a confident verdict if the evidence is mixed, say so.

5. WHAT WOULD CHANGE THE VERDICT: name 1-2 concrete, checkable events (a
   specific type of competitor raise, a specific regulation, a specific cost
   threshold) that would flip the read from "wait" to "go" or vice versa.

Do not cite specific funding rounds, company names, or dates unless you're
confident they're real and roughly current. Where you're inferring rather
than citing something known, say so plainly.

End with a one-line recommendation: enter now, wait for a specific trigger,
or don't enter this market.

Start by asking me for the market or product I'm considering entering, then
wait for my reply before doing anything.
```

## Real example output
```
Market: AI-powered code review tools for individual developers (not
enterprise)

ENABLING SHIFTS
- LLM code-understanding quality crossed a threshold where automated review
  catches real bugs, not just style nits, roughly since GPT-4-class models
  became widely available (SOURCED: broadly reported capability jump, but I
  can't cite one precise benchmark for this)
- IDE/editor extension marketplaces matured enough that distribution to
  individual developers is now cheap and fast, versus needing enterprise
  sales 5 years ago (ESTIMATE, inferred from extension ecosystem growth
  patterns, not a specific data point)
- Developers increasingly already pair with AI coding assistants day to day,
  lowering resistance to adding another AI tool to the workflow (ESTIMATE,
  inference from the broader AI coding assistant adoption trend, no single
  source)

TOO-EARLY SIGNALS: not currently present. This would look like developers
not yet trusting AI output enough to act on it, or false-positive rates
still too high without heavy tuning, true 2-3 years ago, less true now,
though I can't verify current false-positive rates with confidence.

TOO-LATE SIGNALS: partially present. Several well-known AI coding assistants
already bundle review/PR-comment features into a broader subscription
(SOURCED: these are publicly known product features, though I can't verify
their exact current scope without checking current docs). That means a
standalone reviewer competes with a free bundled feature, not just other
standalone tools, this is the biggest risk to entering now.

CURRENT READ: Right on time for the underlying technology and behavior
shift, but likely too late to win with a generic "AI reviews your code"
pitch, since that's becoming a bundled feature. Genuinely uncertain whether
a specific underserved wedge (a language, a bug class, a workflow the
bundled tools handle poorly) still exists.

WHAT WOULD CHANGE THE VERDICT
- Naming a specific bug class or workflow the bundled tools handle poorly
  (would need direct verification, not assumption) flips this from "don't
  enter" to "enter with a narrow wedge"
- A dominant bundled tool announcing the exact narrow feature you planned to
  build is a clear "don't enter" trigger

RECOMMENDATION: Don't enter with a generic pitch. Spend one week validating
whether a specific underserved wedge exists before deciding.
```

## Why it works
Splitting enabling shifts from too-early and too-late signals forces three separate judgment calls instead of one blended gut feeling. Naming a concrete, checkable trigger event turns "we're not sure about timing" into something you can actually monitor and revisit.
