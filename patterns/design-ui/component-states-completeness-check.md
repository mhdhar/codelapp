---
title: "The Missing-States Loop: Catch Every Interactive Component Missing a State"
slug: "component-states-completeness-check"
format: "loop"
category: "design-ui"
tools: ["claude-code", "cursor"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Loop through every interactive component and force it to have every state it needs."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["empty-state-design-pass", "accessible-name-audit-for-custom-controls", "silent-failure-hunt"]
---

## When to use this
A component was built and tested once, in its happy-path default state, and shipped. Use this right after any new interactive component (button, input, select, card-with-action) is built, before it goes near production data.

## The pattern
```text
Take the interactive component you built or changed most recently (or the one I name in
my next message) and check whether it has an explicit, intentionally designed treatment for
every state in this list. "Explicit" means someone decided what it looks like - inheriting
the browser default or silently falling through to the default state does not count.

REQUIRED STATES (check each one, mark PASS/MISSING):
1. Default - the resting state.
2. Hover - for anything clickable on a device with a pointer.
3. Focus-visible - must be visibly distinct from default, must NOT rely only on the browser's
   native outline being left in place by accident (check it wasn't disabled with
   outline: none and never replaced).
4. Active/pressed - the moment of click/tap, not the same as hover.
5. Disabled - visually distinct from default, and confirm it's actually non-interactive
   (not just grayed out CSS while the click handler still fires).
6. Loading - if the component triggers any async action, is there a state for "in flight"?
7. Error - if the component can fail (a form field, a fetch-triggering button), is there a
   state for that, distinct from disabled?
8. Empty - if the component displays data, what does it look like with zero items?

For every MISSING state, do not just describe it - implement it in the component now, using
existing tokens/colors from the project's design system if one exists. After implementing,
re-run this same checklist against the updated component and confirm all applicable states
are now PASS. Show the checklist result for both rounds.
```

## Real example output
```
Component: SubmitFormButton (components/submit/SubmitFormButton.tsx)

ROUND 1
1. Default: PASS - solid accent background, white text.
2. Hover: PASS - darkens 10%.
3. Focus-visible: MISSING - outline: none is set globally on buttons, no replacement ring
   added on this component.
4. Active/pressed: MISSING - no distinct state, hover and active look identical.
5. Disabled: MISSING - the button has no disabled prop at all; it's clickable during
   submission, so double-submit is possible.
6. Loading: MISSING - same issue as above, no in-flight indicator, button just sits there
   while the request is pending.
7. Error: PASS - parent form shows an error banner on failed submission.
8. Empty: N/A - button has no data-display role, doesn't apply.

5 of 7 applicable states MISSING.

FIXES APPLIED:
- Added focus-visible:ring-2 focus-visible:ring-accent focus-visible:ring-offset-2.
- Added active:scale-[0.98] for a pressed state distinct from hover.
- Added disabled prop wired to isSubmitting, with disabled:opacity-50
  disabled:cursor-not-allowed, and confirmed the onClick handler checks isSubmitting before
  firing (not just visual).
- Added a loading state: button text swaps to "Submitting..." with a small spinner, disabled
  is true for the same duration.

ROUND 2
1. Default: PASS  2. Hover: PASS  3. Focus-visible: PASS (ring visible, tab-tested)
4. Active: PASS  5. Disabled: PASS (verified click handler no-ops while isSubmitting)
6. Loading: PASS  7. Error: PASS  8. N/A

All applicable states now PASS.
```

## Why it works
A component demoed once in a browser only ever shows its default state, so missing hover, focus, disabled, or loading treatments never surface until a real user tabs through the form or double-clicks submit. Naming all 8 states as a fixed checklist catches the ones that are easy to forget - disabled and loading especially - because "does it look right" only ever tests the state you happened to be looking at.
