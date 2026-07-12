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
related: ["headline-subject-line-variants", "tighten-draft-to-word-limit", "strip-ai-voice"]
---

## When to use this
Your draft's first paragraph is warming up instead of starting: scene-setting, throat-clearing, restating the title as a sentence, and the actual interesting claim is buried three lines down.

## The pattern
```text
Look at my draft opening. Find the sentence where the actual point,
tension, or specific claim first appears, not scene-setting or
restating the topic.

If that sentence isn't the first sentence:
1. Show me which sentence it is and how many sentences come before it.
2. Rewrite the opening so that sentence, or a tightened version of it,
   comes first. Fold in anything from the earlier sentences that's
   actually load-bearing for context, don't just delete it if it's
   needed.
3. Give me 2 alternate first lines using different hooks: one that opens
   on a specific supplied detail or number, one that opens on tension or
   contradiction already present in the paragraph. If the paragraph has no
   support for a detail-first hook, write `NEEDS DETAIL:` and name the fact
   needed instead of inventing one.

If the opening already leads with the point, say so and don't rewrite it.

First ask me to paste the first paragraph, then wait. Don't start
until you have it.
```

## Real example output
Original: "In today's fast-moving software landscape, teams are constantly looking for ways to improve their development workflows. One area that often gets overlooked is the code review process. At our company, we recently made a change to how reviews work that had a significant impact."

The actual point is sentence 3, buried behind two sentences of throat-clearing.

Rewrite: "At our company, we changed how code review works, and it had a significant impact."

Alt 1 (detail-first): "NEEDS DETAIL: add a real before/after review metric or observed outcome."

Alt 2 (tension-first): "Code review was the overlooked part of our development workflow, so we changed how it works."

## Why it works
Naming the specific tell, context-setting sentences before the real claim, gives the model something concrete to hunt for instead of the vague ask "make this hook better," which usually just produces a punchier version of the same throat-clearing. Forcing two different hook mechanisms as alternates stops it from handing back only one option dressed up as a rewrite.
