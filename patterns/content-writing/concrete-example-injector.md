---
title: "Find Every Abstract Claim and Force a Concrete Example"
slug: "concrete-example-injector"
format: "prompt"
category: "content-writing"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Flag every vague claim in a draft that has no real example backing it, then fill or cut it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["fact-claim-audit-against-notes", "skeptical-reader-gauntlet", "ai-citation-worthiness-audit"]
---

## When to use this
A draft reads fine sentence by sentence but leaves no specific memory after reading it, usually because it's full of confident-sounding abstractions like "this dramatically improves efficiency" with no concrete example anywhere near them.

## The pattern
```text
Find every abstract or generic claim in my draft: a claim that could
appear in almost any piece about almost any product without changing
meaning (e.g. "this saves time," "customers love it," "this scales
well").

For each one:
1. Quote the sentence.
2. Check my source material for a specific example, number, or
   anecdote that could replace or support it.
3. If you find one, rewrite the sentence with that specific detail in.
4. If you don't find one, leave the original sentence and add
   (NEEDS EXAMPLE: what kind of detail would fix this) right after it.
   Do not invent an example that isn't in my source material.

Don't touch sentences that are already specific (named numbers, named
people, named events). Only act on genuinely generic claims.

First ask me in one message for both: (1) the draft, (2) the source
material (real examples, data, quotes). Then wait, don't start until
you have both.
```

## Real example output
"This makes the whole review process much faster." → Source material has: "Sarah's team went from a 3-day average review turnaround to same-day." Rewrite: "This is what got Sarah's team from a 3-day average review turnaround to same-day."

"Teams that adopt this see a real improvement in code quality." → No supporting detail found in source material. Left as-is, flagged: (NEEDS EXAMPLE: a before/after defect count, or a specific bug class this catches, would fix this)

"The API responds in under 200ms for the search endpoint, even at 10k requests per second." → Untouched, already specific.

## Why it works
Naming what makes a claim "abstract," interchangeable across any product, gives the model a real filter instead of a vibe check. Refusing to invent examples is what keeps this from just producing more convincing-sounding fabrication. The NEEDS EXAMPLE flag turns a hidden weakness into a visible todo you can actually go fill.
