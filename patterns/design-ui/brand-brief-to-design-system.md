---
title: "Turn a Rough Brand Brief Into a Design System the Agent Can Apply Consistently"
slug: "brand-brief-to-design-system"
format: "workflow"
category: "design-ui"
tools: ["claude-code", "cursor"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Feed it three colors and a vibe, get back a concrete palette, type scale, and spacing scale to reuse everywhere."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You have a rough sense of the brand (a couple of colors, a tone, maybe a reference site) but no actual design system, and every new component the agent builds picks its own shade of blue and its own padding. Use this once, early in a project, then reference the output file in every future UI prompt.

## The pattern
```text
Act as a design systems lead. Turn my rough brand direction into a concrete, codeable
design system. Do not just describe the system, output exact values.

Output a single markdown file named DESIGN_SYSTEM.md with these exact sections:

1. Color palette: primary, secondary, accent, neutral scale (50-900), success/warning/error.
   Give hex values, not names.
2. Type scale: font family choice with a real name (not "a modern sans"), and a 6-step
   size scale (display, h1, h2, body, small, caption) with px and line-height for each.
3. Spacing scale: an 8-step scale in px, name each step (xs through 3xl).
4. Border radius scale: 3 values (sharp, default, pill) with px.
5. One rule at the bottom: "When building any UI component, pull every color, size, and
   spacing value from this file. Never invent a new value inline."

After you generate it, tell me one place in the existing codebase where current
styles violate this system, as a sanity check.

Before you do anything, ask me for these three in one message and wait for my reply:
(1) primary color or mood (e.g. deep forest green, trustworthy but not corporate),
(2) reference sites or products for tone (e.g. Linear, Stripe docs), (3) personality
in 3 words (e.g. precise, calm, a little unusual). If I skip any of them, infer a
defensible direction from the product itself (read the README and any existing UI)
and say what you assumed.
```

## Real example output
```
DESIGN_SYSTEM.md created.

## Colors
Primary: #1B4332 (deep forest)
Secondary: #2D6A4F
Accent: #D4A017 (warm gold, used sparingly for CTAs)
Neutral: 50 #F7F8F7, 200 #DDE2DE, 500 #6B7570, 700 #33382F...
Success #2D8A5C  Warning #B8860B  Error #B23A34

## Type
Font: Inter (body), Source Serif 4 (display)
Display 48/56  H1 36/44  H2 24/32  Body 16/24  Small 14/20  Caption 12/16

## Spacing
xs 4  sm 8  md 16  lg 24  xl 32  2xl 48  3xl 64

## Radius
sharp 2px  default 8px  pill 999px

Sanity check: Button.tsx currently uses #2563EB for its primary state,
which isn't in this palette. Should be #1B4332 once you apply this system.
```

## Why it works
Vague brand words like "modern" or "clean" produce generic output because the model has nothing concrete to anchor to. Forcing exact hex codes, px values, and named steps into one reference file gives every later prompt something to point at, so "use the design system" actually constrains the output instead of being a suggestion the agent forgets by the third component.
