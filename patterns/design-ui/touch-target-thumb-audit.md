---
title: "The 44-Pixel Rule: Measure Every Touch Target Before Mobile Users Do"
slug: "touch-target-thumb-audit"
format: "goal"
category: "design-ui"
tools: ["universal"]
difficulty: "beginner"
est_time: "15 min"
models: ["any"]
summary: "Measure tap targets in an agreed mobile route/state scope, fix confirmed misses, and show a before/after table."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["responsive-breakpoint-stress-test", "highest-impact-a11y-fix-loop", "accessible-name-audit-for-custom-controls"]
---

## When to use this
The app was built and tested with a mouse, and now mobile users are missing taps: icon buttons that take three tries, links so close together they hit the wrong one. Use this before any mobile launch, or whenever a design review keeps saying "feels fiddly on the phone" without anyone measuring why.

## The pattern
```text
First ask me for the routes and states to cover, plus whether I want AUDIT ONLY or
AUDIT + FIX. If no scope is given, audit one public route and list all exclusions.

GOAL: every interactive element in the agreed scope has a hit area of at least 44x44px on
mobile, measured on the rendered page, not guessed from the CSS.

SUCCESS CRITERIA (all must hold before you report done):
1. You've inventoried every interactive element type in the agreed scope: buttons, links, icon
   buttons, checkboxes, radio buttons, tab triggers, close/dismiss buttons, table row
   actions, pagination controls.
2. At a 375px viewport, you've measured the actual clickable box of each type on the agreed
   routes/states (rendered bounding box plus any padding that receives the tap). Measure icon
   buttons and inline icon links first; they're the usual offenders.
3. Every element under 44x44 is either proposed, fixed, or flagged. In AUDIT ONLY mode, do not
   edit. In AUDIT + FIX mode, fix with padding, min-height/
   min-width, or an expanded pseudo-element hit area, so the visual size stays the same
   and only the tappable box grows. Flag as an accepted exception only with a reason,
   e.g. links inside running body text, where a 44px line-height would be worse.
4. Adjacent targets have at least 8px between their hit areas so one thumb press can't
   land on both. Fix any that touch.
5. You've re-measured after the fixes and produced a before/after table: element, page,
   size before, size after.
6. Re-check the agreed desktop and mobile breakpoints. Report any visual change or untested breakpoint.
```

## Real example output
```
Inventory: 9 interactive element types across 6 routes. Measured at 375px.

| Element             | Page       | Before      | After       |
|---------------------|------------|-------------|-------------|
| Card overflow menu  | /projects  | 24x24       | 44x44 (padding, icon unchanged) |
| Toast dismiss X     | global     | 16x16       | 44x44 (::after hit area)        |
| Pagination arrows   | /search    | 32x36       | 44x44 (min-w/min-h)             |
| Footer social icons | global     | 20x20, 4px apart | 44x44, 12px apart          |
| Checkbox (settings) | /settings  | 18x18       | 44x44 (label wraps input, whole row taps) |

Accepted exceptions: inline links in article body text (marked, reason: enlarged hit
areas overlap across wrapped lines).

Spacing pass: footer icons were the only adjacent pair under 8px, fixed above.
Re-measured all 9 types: smallest hit area is now 44x44. Desktop screenshots at 1440px
match the originals, no visual change.
```

## Why it works
"Feels fiddly" is unactionable, but 44x44 (Apple's HIG floor, near WCAG 2.5.8's 24px minimum with margin to spare) is a number the agent can measure against on the rendered page. Separating hit area from visual size means fixes don't spark a redesign debate, and the before/after table makes the improvement checkable line by line instead of taken on trust.
