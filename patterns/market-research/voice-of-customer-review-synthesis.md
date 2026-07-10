---
title: "Voice-of-Customer Synthesis from Public Reviews"
slug: "voice-of-customer-review-synthesis"
format: "prompt"
category: "market-research"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Turn a pile of pasted reviews for an adjacent product into ranked complaints and unmet needs."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["competitor-complaint-mining", "interview-notes-to-decision-themes", "testimonial-extraction-from-raw-feedback"]
---

## When to use this
You're scoping a product adjacent to an existing category and want to mine what real users already complain about in public reviews (G2, App Store, Amazon, Reddit) for a comparable product, before assuming you already know their pain points.

## The pattern
```text
Act as a voice-of-customer analyst. I'll paste public reviews for a
product adjacent to what I'm building (not a direct competitor, but close
enough that its user complaints are relevant signal).

Using ONLY the text I paste, do not invent or recall reviews you weren't
given, do the following:

1. COMPLAINT THEMES: group recurring complaints into themes, each theme
   needs 2+ reviews to count. Quote directly, word for word, never
   paraphrase into a fake quote.

2. PRAISE THEMES: same, but for what users like, this tells you what NOT
   to break if you're building something adjacent.

3. UNMET NEED SIGNAL: for each complaint theme, say whether it looks like
   (a) something my product idea could directly solve, (b) unrelated to my
   product, or (c) unclear. Don't force every complaint into being relevant.

4. RATING PATTERN: if star ratings were included, note whether complaint
   themes cluster in low-star reviews or also appear in high-star reviews.
   The latter is a stronger signal, it means people who overall like the
   product still hit this wall.

5. GAPS: name what you can't conclude from this review set alone (sample
   size, the review site's bias toward certain complaint types, etc.).

Do not present a theme found in only one review as if it's a pattern. If
the review set is too small or too repetitive to support real themes, say
so instead of manufacturing structure that isn't there.

Start by asking me in one message for: (1) the adjacent product's name,
(2) my product idea, (3) 10-30 raw reviews pasted, with star ratings if
available. Wait for my reply before analyzing anything.
```

## Real example output
```
Adjacent product: Notion (comp for a lighter-weight team wiki I'm building)
My idea: a stripped-down team wiki with less setup overhead than Notion
Reviews: 18 G2/App Store reviews pasted, mix of 2-5 star

COMPLAINT THEMES
1. "Too much setup before it's useful" - 6 reviews
   "took our team two weeks to actually agree on a structure before anyone
   used it for real work" (G2, 4-star)
   "it's powerful but I spent more time building the system than using it"
   (G2, 3-star)
2. "Slow/laggy with large databases" - 4 reviews
   "pages with big tables take a few seconds to load now" (App Store, 3-star)
   "gets noticeably slower once you have a lot of content" (G2, 4-star)
3. "Mobile app is clunky for quick edits" - 2 reviews, WATCH, not a full
   theme yet
   "editing on my phone feels like a second-class experience" (App Store,
   2-star)

PRAISE THEMES
1. "Flexibility once set up" - 7 reviews, the most consistently praised
   feature
   "once we had our system it could do literally anything we needed" (G2,
   5-star)
   "nothing else lets you customize this much" (G2, 5-star)
Note: this directly tensions with complaint theme 1, users like the
flexibility that also causes the setup burden, worth naming plainly rather
than treating it as a simple problem to fix.

UNMET NEED SIGNAL
- Theme 1 (setup overhead): directly solvable by my idea, the clearest
  validation signal in this set
- Theme 2 (performance/lag): unrelated to a "less setup" pitch either way
- Theme 3 (mobile editing): unclear, only 2 mentions, not a pattern yet,
  worth watching, not building for

RATING PATTERN: Theme 1 (setup overhead) appears in both 3-star AND 4-star
reviews, not just low-rated ones, that's a stronger signal, people who
otherwise rate the product well still hit this wall. Theme 2 skews toward
3-star reviews specifically.

GAPS: 18 reviews from two sources is a small sample, and G2 reviewers skew
toward the admin/decision-maker who chose and set up the tool, they may
over-index on setup complaints versus a rank-and-file team member who just
uses whatever's already configured. Can't conclude how a first-time solo
user experiences onboarding from this set alone.
```

## Why it works
The word-for-word quoting rule stops the model from smoothing scattered complaints into a tidier-sounding claim than the reviews actually support. Checking whether a complaint shows up even in high-star reviews, not just low ones, separates a real product gap from ordinary dissatisfied-customer noise.
