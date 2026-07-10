---
title: "Audit, Rank by Harm, Fix One, Re-Verify: The Accessibility Fix Loop"
slug: "highest-impact-a11y-fix-loop"
format: "loop"
category: "design-ui"
tools: ["claude-code", "cursor"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Run automated and manual a11y checks, fix only the highest-impact blocker, then prove it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["accessible-name-audit-for-custom-controls", "touch-target-thumb-audit", "dark-mode-parity-check"]
---

## When to use this
You want a real accessibility pass on a shipped page or app, not a giant backlog of minor nits nobody will action. Use this when you have both automated tooling (axe-core or equivalent) and the ability to drive the page yourself, and you want one confirmed blocker fixed and proven fixed, not fifty flagged.

## The pattern
```text
Run a full accessibility pass on this app against WCAG 2.2 AA. Start the local dev server
if it isn't already running and audit every route; if I want a single page or a deployed
URL instead, I'll say so in my next message. Do this in three passes and do not skip any:

PASS 1 - Automated: run an axe-core (or equivalent) scan on every page/route and on every
theme variant (light and dark, or any theming the app has). List every violation with its
rule id, WCAG success criterion, and every DOM node it fired on.

PASS 2 - Manual keyboard: tab through the entire page using only the keyboard. Note every
control that: can't be reached by tab, has no visible focus indicator, traps focus, or
doesn't respond to the expected key (Enter/Space to activate, arrows to navigate a listbox,
Escape to close).

PASS 3 - Manual screen-reader / accessibility tree: inspect the accessibility tree (or use a
real screen reader) for every interactive control. Note anything announced with no name, the
wrong role, or a name that doesn't match its visible label.

Then:
1. Merge all findings from the three passes into one list. Do not double-count the same root
   cause found by two passes.
2. For each finding, confirm it's real by re-checking the actual DOM/code, not just trusting
   the tool output - tools produce false positives.
3. Rank confirmed findings by harm: does it block a user from completing the task at all
   (critical), or does it degrade the experience but leave a workaround (moderate/minor)?
4. Fix ONLY the highest-ranked finding. Do not batch-fix everything in one pass.
5. After the fix, re-run PASS 1, 2, and 3 exactly as before, scoped to the area you changed
   at minimum. Confirm: the original violation is gone, the automated scan shows 0 new
   violations, and the manual keyboard/screen-reader check still works end-to-end for that
   control (can still be reached, operated, and understood).
6. Report the before/after violation count and the specific fix. Only then ask whether to
   continue to the next-ranked finding.
```

## Real example output
```
PASS 1 - Automated (axe-core, all routes, light + dark)
Scanned: /, /patterns, /patterns/[category], /submit, /about - light and dark theme (10 scans)
Result: 1 violation, 2 instances, both on /submit (light and dark identical)
- rule: button-name (critical) - WCAG 4.1.2 Name, Role, Value
  nodes: Select.Trigger[data-field="category"], Select.Trigger[data-field="format"]

PASS 2 - Manual keyboard, all routes
Tab order correct on every page. Focus ring visible on all interactive elements. Both Select
triggers on /submit ARE reachable and operable by keyboard (tab focuses, Enter opens, arrows
move, Enter selects) - so this is a naming problem, not an operability problem.

PASS 3 - Accessibility tree, /submit
CategorySelect trigger: role=combobox, name="" (empty)
FormatSelect trigger: role=combobox, name="" (empty)
Every other control on every page: name matches visible label.

MERGED FINDINGS: 1 confirmed issue - both instances share the same root cause (Select
trigger not linked to its <Label>). Nothing else found across 10 scans + 5 manual passes.

RANK: Critical (WCAG 4.1.2, blocks screen reader users from knowing what either field is for
before submitting the form) - highest and only ranked item.

FIX: Added aria-labelledby on both Select.Trigger components, pointing at the existing
Label's id. No new elements added, no visual change.

RE-VERIFY:
PASS 1 rerun: 0 violations across all 10 scans (was 1 violation / 2 instances).
PASS 2 rerun on /submit: tab → focus lands on CategorySelect trigger → Enter opens listbox →
arrow down moves through options → Enter selects "SEO & GEO" → trigger now shows and
announces "Category: SEO & GEO". Same sequence confirmed on FormatSelect. Operability
unchanged, naming now correct.
PASS 3 rerun: role=combobox, name="Category" / name="Format". Matches visible labels.

No further findings. Nothing else to rank. Shipping.
```

## Why it works
Automated scanners catch missing names and roles but not missing operability, and manual keyboard/tree checks catch the opposite - running only one class of check leaves half the WCAG surface unverified. Ranking by harm before fixing stops you from burning the session on minor label-wording nits while a critical blocker ships, and re-running the identical checks (not a new spot check) is the only way to prove the fix didn't silently break what already worked.
