---
title: "Cut a Draft to a Hard Word Limit Without Losing the Point"
slug: "tighten-draft-to-word-limit"
format: "goal"
category: "content-writing"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Hit an exact word or character limit by cutting weak sentences first, not shrinking every sentence a little."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You have the right content in the wrong length: a post over the character cap, an exec summary that needs to fit on one screen, an abstract with a hard word count. Naive trimming makes every sentence a little worse instead of cutting the weak ones entirely.

## The pattern
```text
Cut the draft I paste below to exactly the limit I give at the bottom,
not "close to it."

Process:
1. Rank every sentence in the draft by how much it would hurt to lose,
   from "essential to the argument" to "restates a point already made."
2. Cut whole sentences starting from the weakest, not by trimming words
   out of every sentence. A shorter draft with full sentences reads
   better than a same-length draft where every sentence got clipped.
3. After each cut, recheck: does the draft still make its core point,
   and is it still true to the original meaning? If a cut breaks the
   logic (a conclusion now has no setup), restore that sentence and cut
   a different one instead.
4. Stop as soon as you hit the limit. Do not keep cutting past it, and
   do not stay padded above it.
5. Report the final word or character count, then list what you cut, one
   line each.

Limit (words or characters):
Paste the draft below this line:
```

## Real example output
Original (326 characters, target 190 for a post caption): "When you're hiring your first support engineer, it's tempting to look for someone who already knows your product. That's the wrong instinct. What actually matters is whether they can sit with an angry customer for twenty minutes and get to the real problem instead of the stated one. Product knowledge you can teach in a week."

Result (188 characters): "Don't hire your first support engineer for product knowledge, that's teachable in a week. Hire for the instinct to sit with an angry customer and find the real problem, not the stated one."

Cuts: dropped "That's the wrong instinct" (redundant with the sentence that follows it), merged the opening into a direct instruction instead of two separate sentences.

## Why it works
A concrete target with "report the final count" turns a vague ask into something the model can actually check itself against instead of eyeballing "shorter." Ranking sentences by how much they'd hurt to lose, instead of trimming every sentence uniformly, is what produces a draft that reads clean at the new length instead of one where every sentence lost a limb.
