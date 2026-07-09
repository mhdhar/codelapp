---
title: "Dark Mode Parity Check: Find Where the Second Theme Was an Afterthought"
slug: "dark-mode-parity-check"
format: "prompt"
category: "design-ui"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Walk every page in both themes and catch the contrast failures light mode was hiding."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The app supports light and dark themes, light mode was clearly the one designed first, and you suspect dark mode has never actually been checked page by page. Use this before shipping any theme-aware UI, not after a user reports illegible text.

## The pattern
```text
Walk every page/route in [APP NAME] in both light and dark theme and check for theme parity.
Do not just glance at the color scheme — actively check these failure modes on each page,
in each theme:

1. Contrast: any text that's hard to read against its background in dark mode specifically
   (common cause: a color hardcoded as a fixed hex instead of a theme token, so it doesn't
   flip with the theme).
2. Dead/invisible elements: borders, dividers, or icons that were visible in light mode but
   disappear or blend into the background in dark mode (common cause: a light gray border
   color that has no dark-mode equivalent).
3. Wrong-direction color: anything that looks inverted or broken specifically because it
   uses a raw color value (e.g. "white card") instead of a semantic token (e.g. "surface"),
   so it renders as a literal white card floating on a dark page instead of a themed surface.
4. Images/illustrations/logos: anything with a transparent background or baked-in color
   that assumed a light backdrop and now looks wrong or illegible in dark mode.
5. Focus rings and status colors (success/error/warning): confirm they're still visibly
   distinct from the background in both themes, not just present.

For every failure, name the exact file and the exact hardcoded value causing it, and replace
it with the correct theme token. Report a page-by-page list: page name, failures found in
dark mode, fix applied. Pages with no failures still get listed as "checked, no issues."
```

## Real example output
```
Checked all 5 routes in both themes:

/ (homepage) — checked, no issues.
/patterns — checked, no issues.
/patterns/design-ui — DARK MODE FAIL: category badge border invisible.
  Cause: CategoryBadge.tsx used border-gray-200 (a light-mode-only Tailwind class) instead
  of the theme-aware border-border token already defined in the project.
  Fix: replaced with border-border, now visible as a subtle line in both themes.
/submit — checked, no issues (already uses theme tokens throughout).
/about — DARK MODE FAIL: team photo section has a white background rectangle behind a
  logo with transparent PNG, rendering as a glaring white box on the dark page.
  Cause: logo container used bg-white hardcoded instead of no background / bg-surface.
  Fix: removed the hardcoded white background, logo now sits directly on the themed
  surface color which already has enough contrast for the logo's dark line art.

2 pages with dark-mode-only failures found and fixed, out of 5. Both root causes were
hardcoded light-mode-only values instead of theme tokens.
```

## Why it works
Light mode is almost always built and eyeballed first, so any hardcoded color that happens to look fine against a white background ships invisibly broken and nobody notices until dark mode is checked page by page. Naming the specific failure modes — dead borders, inverted cards, illegible focus rings — gives the model something concrete to hunt for instead of a vague "check dark mode too" that it skims past.
