---
title: "Design a Real Empty State, Not a Blank Screen"
slug: "empty-state-design-pass"
format: "prompt"
category: "design-ui"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Turn every '0 results' screen in the app into a state that explains, guides, and invites action."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're auditing a page or flow that has a zero-data condition — no results, an empty inbox, a first-time user, a cleared filter — and it currently renders as a blank area or a lonely "No data" string. Use this before shipping any list, table, or dashboard view.

## The pattern
```text
Find every empty state in [PAGE OR FEATURE] — any place the UI renders when there is zero
data: no results from a search, an empty list before the user adds anything, a cleared
filter, a first-run/onboarding screen with nothing saved yet.

For each one you find, check whether it currently does all four of these. If it's missing
any, rewrite the component to add it:

1. States what's actually going on, specific to the situation, not a generic "No data"
   or "Nothing here." Distinguish "you have zero items ever" from "your filters matched
   zero of your N items" — these need different copy.
2. Gives the user one clear next action (a button or link), not just an explanation. If
   the empty state is a dead end with no way forward, that's the bug.
3. Matches the visual weight of the page around it — not a giant illustration on an
   otherwise dense, functional page, and not a bare gray sentence on an otherwise
   polished marketing-style page.
4. Renders correctly in every state that leads to it: zero-ever, filtered-to-zero, and
   error-while-loading should not all show the same message if the underlying cause differs.

List each empty state found, what it currently shows, and the rewritten copy + action.
```

## Real example output
```
Found 2 empty states in the /patterns page:

1. Category page with zero patterns (e.g. a category still being filled)
   BEFORE: blank grid, no text at all.
   AFTER: "No patterns published in Market Research yet — 10 are in review." + button
   "Browse all categories" linking to /patterns. Distinguishes "not built yet" from "you
   searched wrong."

2. Search/filter combo that matches nothing (e.g. filtering by tool=cursor + difficulty=
   advanced with no overlap)
   BEFORE: same blank grid as #1, no distinction.
   AFTER: "No patterns match Cursor + Advanced yet." + "Clear filters" button that resets
   to the unfiltered list, plus a link "Suggest this pattern" to the submission form.

Both now use the same small inline icon + one-line copy + single button treatment already
used elsewhere on the site, instead of introducing new visual weight.
```

## Why it works
A blank screen reads as broken, and a generic "No data" message answers the wrong question — it confirms there's nothing without saying why or what to do next. Splitting "zero ever" from "zero after filtering" forces the copy to match the actual cause, which is what turns a dead end into a state the user can act on.
