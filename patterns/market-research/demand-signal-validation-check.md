---
title: "Demand-Signal Validation Before You Build"
slug: "demand-signal-validation-check"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Check search, forum, and workaround-tool signals for real demand before writing a line of code."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You have a product idea and want a cheap sanity check on real demand, using signals you can gather yourself (search interest, forum complaints, existing workarounds), before committing build time.

## The pattern
```text
Act as a demand-validation analyst. I'm considering building this:

[PRODUCT IDEA]

For the target user: [WHO]

Here's what I have on my end. I'll paste it, then you help me interpret it:

SEARCH SIGNAL: [PASTE SEARCH VOLUME DATA / TRENDS DESCRIPTION, OR "not checked yet"]
FORUM/COMMUNITY SIGNAL: [PASTE RELEVANT THREADS OR DISCUSSIONS, OR "not checked yet"]
EXISTING WORKAROUNDS: [DESCRIBE HOW PEOPLE SOLVE THIS TODAY, OR "not checked yet"]

Do the following:

1. INTERPRET WHAT I GAVE YOU: for each signal provided, say what it actually
   indicates, not what I want it to indicate. If search volume is low but
   forum complaints are frequent and specific, say so plainly rather than
   picking the more favorable read.

2. WORKAROUND STRENGTH TEST: a duct-taped workaround (spreadsheet, Zapier
   chain, manual process, a paid point-tool) is usually a stronger demand
   signal than search volume, since it proves someone already paid in time
   or money to solve this. Rate the workaround evidence as WEAK / MODERATE /
   STRONG evidence of real demand, and say why.

3. GAPS: name the signal categories I did NOT give data for (mark them "not
   checked yet" if I said so) and say exactly what to go look for in each,
   e.g. "search Reddit for [specific term]" or "check [specific competitor
   type]'s reviews for complaints about this."

4. FALSE-POSITIVE CHECK: name the most likely way this signal set is
   misleading me (e.g. forum complaints are a vocal minority, or search
   volume is mostly people looking for a free/DIY answer, not a paid tool).

5. VERDICT: based only on what's actually in front of you, not general
   optimism about the idea, say whether the evidence justifies a build,
   needs one more specific check first, or doesn't yet support building.

Do not treat an absent signal as either a good or bad sign, mark unchecked
things as "unknown," not as evidence either way.
```

## Real example output
```
Product idea: auto-generates changelog entries from git commit history for
small dev teams
Target: solo/small-team developers who write changelogs by hand or skip them

SEARCH SIGNAL: not checked yet
FORUM/COMMUNITY SIGNAL: 6 Reddit/HN threads pasted (r/webdev, HN "Ask HN"
  style posts), all variations of "how do you handle changelogs" or "I
  always forget to write mine"
EXISTING WORKAROUNDS: several commenters use conventional-commits + a script,
  or a GitHub Action that greps commit messages, one mentions paying for a
  tool in the "Changelogfy" category

INTERPRET WHAT I GAVE YOU: the forum signal is a real, recurring complaint
pattern (6 threads, consistent framing: "I forget," "it's tedious," "I want
it automated"), not a one-off. That's a genuine pain signal. No search
volume was given, so I can't speak to top-of-funnel demand size, only to
complaint intensity among people already in developer forums.

WORKAROUND STRENGTH TEST: MODERATE-TO-STRONG. Multiple people already built
or paid for a partial solution (custom scripts, a GitHub Action, a named
paid tool). That's stronger than search interest alone. Not STRONG only
because I can't see how many abandoned the DIY script after building it
versus still using it happily.

GAPS
- Search volume: not checked. Look up "changelog generator," "automatic
  changelog," "conventional commits changelog" in a keyword tool or Google
  Trends for search-side demand, not just forum-side.
- Competitor reviews: not checked. If "Changelogfy"-style tools exist, check
  their reviews for pricing objections or feature gaps, that tells you if
  there's room to win share, not just whether demand exists.
- Willingness to pay: not checked. Forum complaints are about tedium, not
  necessarily about wanting to pay for a fix, this data doesn't distinguish
  the two.

FALSE-POSITIVE CHECK: the most likely trap is that this audience (Reddit/HN
developers) skews toward wanting a free open-source script over a paid tool,
since the DIY workarounds mentioned are free. The complaint volume may
indicate demand for "someone to solve this," not demand for "a paid
product," those are different things and this data doesn't distinguish them.

VERDICT: Not yet enough to justify a full build. The pain is real and
recurring, but the workaround evidence raises a real risk that this audience
wants a free script, not a paid product. Next specific check: look at
whether any existing paid changelog tool has meaningful paying customers
(site logos, pricing page maturity, G2/Capterra reviews) before committing
build time.
```

## Why it works
Ranking a duct-taped workaround above search volume as a demand signal reflects how people actually behave: they build spreadsheets for problems that annoy them, they only search for problems they've already decided to solve. Marking unchecked signals as "unknown" instead of guessing keeps the verdict honest about what you don't know yet.
