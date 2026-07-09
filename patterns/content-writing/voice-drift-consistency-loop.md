---
title: "Fix Voice Drift Across a Long Document"
slug: "voice-drift-consistency-loop"
format: "loop"
category: "content-writing"
tools: ["universal"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Loop section by section against a reference paragraph until a long doc reads like one writer wrote it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You drafted a long document across multiple sessions, or stitched together several AI passes, and now section 3 sounds like a different person wrote it than section 1. You need it to read as one consistent voice before it ships.

## The pattern
```text
Below this prompt I'll paste a REFERENCE paragraph that has the voice I
want, then the FULL DOCUMENT to check against it. Loop through this
process:

1. Read the reference paragraph and describe its voice in 3-4 traits
   (sentence length, formality, use of "I" vs "we", contraction use, how
   it handles technical terms).
2. Go section by section through the full document. For each section,
   compare it against those traits. Mark PASS or DRIFT, and if DRIFT,
   quote the exact sentence that breaks voice and say which trait it
   violates.
3. Rewrite only the sections marked DRIFT so they match the reference
   voice. Do not touch sections marked PASS. Do not change facts,
   numbers, or claims, only sentence style and tone.
4. Re-run step 2 on the rewritten sections only.
5. Repeat steps 3-4 until every section passes. Show me the DRIFT list
   for each round, not just the final clean version.

Paste the paragraph whose voice is correct after REFERENCE and the
whole document after FULL DOCUMENT.

REFERENCE:

FULL DOCUMENT:
```

## Real example output
```
ROUND 1
Reference voice: short sentences, contractions used ("it's", "don't"),
first person singular, no hedging, technical terms defined inline not
footnoted.

Section 2: PASS
Section 3: DRIFT — "It should be noted that the implementation may
potentially introduce some overhead in certain scenarios." Violates:
hedging (reference states things directly), no contractions, third-person
distancing ("It should be noted").
Section 5: DRIFT — "We believe this represents a significant step forward
for our platform." Violates: uses "we" instead of "I", generic corporate
phrasing not present in the reference.

Rewriting Section 3: "This adds overhead in a few specific cases, and
I'll call those out below."
Rewriting Section 5: "This is a real step up for what the platform can do."

ROUND 2
Section 3: PASS
Section 5: PASS
All sections pass.
```

## Why it works
Giving the model one concrete reference paragraph instead of an abstract instruction like "keep the voice consistent" gives it something to measure against. Isolating fixes to only the DRIFT sections stops it from rewriting parts that already work, which is what "just make it consistent" prompts usually do instead: rewrite everything and introduce new drift.
