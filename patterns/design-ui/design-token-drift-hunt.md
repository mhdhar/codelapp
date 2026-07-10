---
title: "Token Drift Hunt: Zero Hardcoded Style Values, Proven by Grep"
slug: "design-token-drift-hunt"
format: "goal"
category: "design-ui"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "intermediate"
est_time: "25 min"
models: ["any"]
summary: "Hunt every hardcoded hex and px that bypasses your tokens, replace it, and prove drift is zero with grep."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["brand-brief-to-design-system", "dark-mode-parity-check", "pixel-diff-css-refactor-loop"]
---

## When to use this
The project has a design system (tokens, a Tailwind theme, CSS custom properties) but months of quick fixes have scattered raw hex codes and one-off pixel values through the components. Every new surface drifts a little further from the palette. Use this as a periodic cleanup, or right before a redesign so the new tokens actually take effect everywhere.

## The pattern
```text
GOAL: no component in this codebase uses a hardcoded style value where a design token
exists for it. "Done" is proven by search output, not by eyeball.

First, locate the source of truth: the token or theme definition this project actually
uses (tailwind config, CSS custom properties, a theme.ts, a DESIGN_SYSTEM.md). If there
is no source of truth at all, stop and tell me. That's a different problem and inventing
tokens mid-cleanup would make it worse.

SUCCESS CRITERIA (all must hold before you report done):
1. You've run a project-wide search over component and style code for raw values: hex
   colors, rgb()/hsl() literals, raw px font sizes, and arbitrary spacing values.
   Exclude the token definition files themselves, generated files, and vendored code.
2. Every hit is classified as DRIFT (a token exists for this value, or an obvious
   nearest token does) or LEGITIMATE (truly one-off, like a 1px hairline or a
   third-party embed constraint), with a one-line reason per LEGITIMATE hit.
3. Every DRIFT hit is replaced with the nearest existing token. If a raw value sits
   between two tokens, pick the closer one and note the visual delta. Never mint a new
   token without asking me first.
4. Re-running the exact searches from step 1 now returns only the LEGITIMATE list, and
   you show me that final output verbatim.
5. Nothing visibly broke: open the three pages with the most replacements and confirm
   they still render correctly in both themes if the app has two.

Report: hit count before, the DRIFT vs LEGITIMATE split, files touched, the visual
deltas you accepted in step 3, and the final search output.
```

## Real example output
```
Source of truth: tailwind.config.ts (colors + spacing) and app/globals.css (semantic
CSS variables).

Search before: 41 hits across 17 files.
  grep -rn '#[0-9a-fA-F]\{3,8\}' app components --include='*.tsx' -> 23
  grep -rn 'rgb\|hsl(' -> 6
  grep -rnE '\b[0-9]+px' (in className/style props) -> 12

Classification: 34 DRIFT / 7 LEGITIMATE.
LEGITIMATE kept: 1px hairline dividers (4 hits), Stripe iframe min-height 320px (1),
OG-image generator colors that never render in-app (2).

Replacements (sample):
  components/Callout.tsx: #FEF3C7 -> bg-warning-subtle (exact match)
  components/Nav.tsx: padding: 18px -> p-4 (16px, accepted -2px delta, noted)
  components/StatCard.tsx: color: #6B7280 -> text-muted-foreground (exact)

Search after: 7 hits, all on the LEGITIMATE list. Output pasted above.
Spot-check: /dashboard, /settings, /pricing render clean in light and dark.
34 drift values removed, 12 files touched, 2 accepted sub-3px spacing deltas.
```

## Why it works
"Use the design system" fails as an instruction because the agent can't tell how much drift exists or when it's finished. A grep pattern turns drift into a countable quantity with a testable zero, and the classify-before-replace step stops the agent from steamrolling values that are intentionally off-system. The final verbatim search output is the receipt: either the list is empty or it isn't.
