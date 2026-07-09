---
title: "Rewrite a Throat-Clearing Opening Into a Real Hook"
slug: "opening-hook-rewrite"
format: "prompt"
category: "content-writing"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Fix the first two sentences that make readers bounce before the actual point shows up."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Your draft's first paragraph is warming up instead of starting: scene-setting, throat-clearing, restating the title as a sentence, and the actual interesting claim is buried three lines down.

## The pattern
```text
Look at the opening of this draft. Find the sentence where the actual
point, tension, or specific claim first appears, not scene-setting or
restating the topic.

If that sentence isn't the first sentence:
1. Show me which sentence it is and how many sentences come before it.
2. Rewrite the opening so that sentence, or a tightened version of it,
   comes first. Fold in anything from the earlier sentences that's
   actually load-bearing for context, don't just delete it if it's
   needed.
3. Give me 2 alternate first lines using different hooks: one that opens
   on a specific detail or number, one that opens on tension or
   contradiction.

If the opening already leads with the point, say so and don't rewrite it.

Draft opening (first paragraph only):
[PASTE FIRST PARAGRAPH]
```

## Real example output
Original: "In today's fast-moving software landscape, teams are constantly looking for ways to improve their development workflows. One area that often gets overlooked is the code review process. At our company, we recently made a change to how reviews work that had a significant impact."

The actual point is sentence 3, buried behind two sentences of throat-clearing.

Rewrite: "We changed one thing about how code review works here, and it cut our average time-to-merge in half."

Alt 1 (detail-first): "Our reviews used to take 3 days on average. Now they take 4 hours."

Alt 2 (tension-first): "Most 'improve code review' advice tells you to review faster. We got faster results by reviewing less."

## Why it works
Naming the specific tell, context-setting sentences before the real claim, gives the model something concrete to hunt for instead of the vague ask "make this hook better," which usually just produces a punchier version of the same throat-clearing. Forcing two different hook mechanisms as alternates stops it from handing back only one option dressed up as a rewrite.
