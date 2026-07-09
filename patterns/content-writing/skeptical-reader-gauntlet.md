---
title: "The Skeptical Reader Gauntlet"
slug: "skeptical-reader-gauntlet"
format: "loop"
category: "content-writing"
tools: ["universal"]
difficulty: "advanced"
est_time: "25 min"
models: ["any"]
summary: "Loop hostile-reader objections against your draft, patch the real ones, repeat until it survives a clean pass."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The draft matters: a launch post, a position piece, an announcement people will pick apart. You've read it so many times you can't see the holes anymore, and you'd rather hear the objections now than in the comments.

## The pattern
```text
Run the draft I paste below through a skeptical-reader loop until it
survives a clean pass.

1. Infer the most plausibly hostile reader for this piece: an expert in
   the topic, short on time, allergic to hype, inclined to disagree.
   State who you're playing in one line.
2. Objection pass. Read the whole draft as that reader. List every
   objection: quote the exact sentence, state the objection in the
   reader's voice, rate severity 1-3.
   - 3: would make this reader stop reading or stop trusting me
   - 2: weakens the argument but they'd keep reading
   - 1: nitpick
3. Patch every severity 2 and 3. Legal moves: add evidence that's
   already in the draft or that I've given you, concede the point
   explicitly and narrow the claim, or cut the claim entirely. Illegal
   move: hedging the sentence into mush so the objection technically
   no longer lands. If a patch needs a fact I haven't given you, ask
   me instead of inventing one.
4. Re-run the objection pass on the full draft, not just the patched
   parts. Patches create new weak points.
5. Stop when a full pass produces no new severity 2 or 3 objections,
   or after 4 rounds. If you hit 4 rounds, tell me which objection
   keeps surviving, that's usually a real flaw in the argument, not
   the writing.

Show me each round's objection list as you go, not just the final
draft.

Paste the draft below this line:
```

## Real example output
```
ROUND 1 (playing: a staff engineer who has seen three "we fixed
on-call" posts this year)
- "Our new alerting cut noise by 90%" — severity 3: "Measured how?
  Over what period? 90% of what baseline?" Patched: "cut paged alerts
  from 41 to 4 per week, measured over 6 weeks."
- "Engineers love the new rotation" — severity 2: "Two people said
  this in Slack, or you surveyed?" Cut, replaced with the survey
  number I supplied: 8 of 11 preferred it.
- "This works for any team" — severity 3: overclaim. Narrowed to
  "teams under ~20 engineers with a single product surface."

ROUND 2
- New severity 2 on the patched opener: "41 to 4 sounds cherry-picked,
  was week 6 a holiday week?" Added the per-week table.

ROUND 3
- No new severity 2-3 objections. Two nitpicks logged, left alone.
Done in 3 rounds.
```

## Why it works
A named hostile persona with a severity scale produces specific, quotable objections instead of generic "consider adding more detail" feedback. Banning the hedge as a patch is the teeth: without it, the loop converges on a draft that offends no one because it claims nothing. Re-reading the full draft each round catches the weak points that patches themselves introduce.
