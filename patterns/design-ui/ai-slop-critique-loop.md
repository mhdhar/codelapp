---
title: "Make It Bold, Not Generic: Self-Critique Loop for AI Slop UI"
slug: "ai-slop-critique-loop"
format: "loop"
category: "design-ui"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Give the agent a checklist of AI-slop tells and make it revise its own UI until it passes."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
The agent just built a UI and it looks like every other AI-generated page: centered card, default blue button, generic sans-serif, evenly spaced everything, no focal point. Use this right after it produces a first draft, before you accept it.

## The pattern
```text
Audit the UI you just built against this checklist. For each item, say PASS or FAIL
with a one-line reason, quoting the actual code or value that triggered the fail.

1. Spacing: is every gap the same size (e.g. everything is 16px or 24px)? FAIL if so.
2. Palette: is it using only default framework colors (default blue, default gray)? FAIL if so.
3. Type scale: is there only one or two font sizes doing all the work? FAIL if so.
4. Hierarchy: could a user tell what to look at first in under 1 second? FAIL if unclear.
5. Layout: is everything centered in a single column with no asymmetry? FAIL if so.
6. Distinctiveness: if you removed the logo, could this be any SaaS product from the
   last 3 years? FAIL if yes.

If ANY item fails, revise the actual code to fix it, then re-run this same checklist
on the revised version. Repeat until all 6 items PASS. Show me the checklist result
for each round, not just the final one.

Component or page to audit: [FILE OR COMPONENT NAME]
```

## Real example output
```
ROUND 1
1. Spacing: FAIL: every element uses gap-6 (24px), no variation for grouping.
2. Palette: FAIL: bg-blue-600 is Tailwind's default, no custom brand color used.
3. Type scale: FAIL: only text-base and text-xl appear, no display size.
4. Hierarchy: FAIL: headline and CTA button are visually the same weight.
5. Layout: FAIL: single centered max-w-md column, no asymmetry.
6. Distinctiveness: FAIL: this is the default shadcn hero pattern.

Revising HeroSection.tsx: tightened related items to gap-2, separated groups with
gap-12, replaced bg-blue-600 with brand accent #D4A017, added a 56px display
headline, offset the image right with a 60/40 split instead of centered.

ROUND 2
1. Spacing: PASS: gap-2 within groups, gap-12 between groups.
2. Palette: PASS: accent color and neutral scale from DESIGN_SYSTEM.md, no default blue.
3. Type scale: PASS: display 56px headline, body 16px, distinct weights.
4. Hierarchy: PASS: headline dominates, CTA is secondary weight, eye moves left to right.
5. Layout: PASS: 60/40 asymmetric split, image bleeds off the right edge.
6. Distinctiveness: PASS: no longer matches a stock template on inspection.

All 6 items pass.
```

## Why it works
Models default to the statistical average of their training data, which is exactly what "generic" means. A checklist that names the specific failure modes (uniform spacing, default palette, flat hierarchy) gives the model something concrete to detect and fix, instead of the vague instruction "make it look better," which it can't act on reliably.
