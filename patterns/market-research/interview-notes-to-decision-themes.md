---
title: "Customer Interview Notes to Decision-Ready Themes"
slug: "interview-notes-to-decision-themes"
format: "loop"
category: "market-research"
tools: ["universal"]
difficulty: "advanced"
est_time: "25 min"
models: ["any"]
summary: "Loop scattered interview notes into ranked themes with quotes, until the same theme stops surfacing."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You've run 8-15 customer interviews and have raw notes or transcript excerpts, and need a small set of decision-ready themes instead of a recap of everything everyone said.

## The pattern
```text
Act as a research synthesis analyst. I'm giving you raw customer interview
notes in batches. I want a running theme list that gets more accurate as
more batches come in, not longer.

My first batch of notes is below the line at the end of this message.

Each round, do the following:

1. Extract every distinct claim, complaint, or need mentioned. Quote the
   specific line where possible. Only quote text that is actually in the
   notes, never invent or paraphrase into a fake quote.

2. Group claims into themes. A theme needs at least 2 independent mentions
   to count. A single mention goes into a "one-off, watch list" bucket, not
   a theme.

3. For each theme, give: the theme name, how many interviews so far mention
   it, 1-2 real quotes, and a confidence label:
   - STRONG: 3+ interviews, independently and consistently framed
   - EMERGING: 2 interviews, or 3+ with inconsistent framing
   - WATCH: a single mention, worth tracking, not acting on

4. Flag anything that contradicts an existing theme rather than smoothing
   it over. A dissenting voice stays visible even if the theme still holds.

5. Show the FULL updated theme list each round, not just what changed.

After you output the round, I will paste the next batch. Keep updating the
same theme list, don't restart it. Say explicitly if a theme should be
downgraded (e.g. STRONG to EMERGING) because new notes contradict or fail
to reinforce it.

When I say "final round": output the finished theme list ranked by
confidence and mention count, drop watch-list items that never became a
theme, and write a 3-sentence decision-ready summary: what to build or fix
first based on theme strength, and what's still too thin to act on.

Paste your first batch of interview notes or transcript excerpts below this line:
```

## Real example output
```
Product: project-management tool for freelance designers/developers
11 interviews total, run across 2 batches

ROUND 1 (5 interviews)
THEMES:
1. Invoicing/payment chasing is the real pain, not task tracking — STRONG,
   4/5
   "I don't need another to-do list, I need someone to chase late payments
   for me" (P2)
   "The task board part I could take or leave, honestly" (P4)
2. Client-facing status pages would cut down "just checking in" emails —
   EMERGING, 2/5
   "Half my emails are just 'where are we at,' I wish they could just check
   themselves" (P1)
WATCH LIST: Toggl integration request (P3), dark mode request (P5)

ROUND 2 (6 more interviews, 11 total)
UPDATED THEMES:
1. Invoicing/payment chasing — STRONG, now 8/11, consistent framing across
   both rounds
   New quote: "I use three different tools right now just to get paid" (P9)
2. Client-facing status pages — upgraded to STRONG, now 5/11
   New quotes from P7, P8
3. NEW: Scope creep tracking — EMERGING, 3/11, first appeared this round
   "I wish I had a paper trail of what was actually agreed vs what they're
   asking for now" (P10)
CONTRADICTION FLAGGED: P11 said "I actually like having tasks and invoicing
in separate tools, less risk if one goes down" — doesn't kill theme 1 (still
8/11 want it combined) but is a real dissent worth keeping visible, not
smoothed over.
WATCH LIST unchanged: Toggl integration (1 mention), dark mode (1 mention)

[user says "final round"]

FINAL THEME LIST (11 interviews)
1. Invoicing/payment chasing — STRONG (8/11)
2. Client-facing status pages — STRONG (5/11)
3. Scope creep tracking — EMERGING (3/11)
Dropped: Toggl integration, dark mode (never exceeded 1 mention)

DECISION SUMMARY: Build invoicing/payment automation first, it's the
strongest and most consistent theme even against one dissenting voice.
Client status pages are a solid second bet. Scope creep tracking is real but
too thin at 3/11 to prioritize engineering time yet, worth asking about
directly in the next 5 interviews.
```

## Why it works
Requiring 2+ independent mentions before something counts as a theme stops one loud interviewee from steering the roadmap. Carrying the same running list across batches instead of restarting each time is what turns a pile of notes into something that gets more confident with more data, not just longer.
