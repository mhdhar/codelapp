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
You want a focused accessibility pass on a page or app, not a giant backlog of minor nits nobody will action. Use this when you have automated tooling (axe-core or equivalent), a way to drive the relevant page, and a defined route/state scope. The outcome is one confirmed blocker fixed and re-verified, not an unsupported claim of full-app compliance.

## The pattern
```text
Run a scoped accessibility check aligned with WCAG 2.2 AA. Before scanning, inspect the app and
write a coverage manifest: routes, auth roles, key states (including empty/error/loading where
relevant), themes, breakpoints, available browser/assistive tooling, and any excluded coverage.
Ask in one message for a target scope and safe test accounts or data only where they are missing.
If I do not name a scope, default to one public route and document every exclusion. Use a local or
safe test environment; do not access production or mutate real user data.

After scope is agreed, start the local dev server if needed and do these three passes for every
route/state/theme in the coverage manifest. Record unavailable checks as unverified; do not skip
them silently:

PASS 1 - Automated: run an axe-core (or equivalent) scan on every manifested route/state/theme.
List every violation with its rule id, WCAG success criterion, and every DOM node it fired on.

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
4. Fix ONLY the highest-ranked finding. This request authorizes one minimal local code fix for a
   confirmed finding; do not batch-fix, deploy, or alter production data.
5. After the fix, re-run PASS 1, 2, and 3 exactly as before, scoped to the area you changed
   at minimum. Confirm: the original violation is gone, the automated scan shows 0 new
   violations, and the manual keyboard/screen-reader check still works end-to-end for that
   control (can still be reached, operated, and understood).
6. Report the coverage manifest, before/after violation count, specific fix, and every remaining
   unverified route/state/tooling gap. Only then ask whether to continue to the next-ranked finding.
```

## Real example output
```
Coverage manifest: public routes only (`/`, `/patterns`, `/patterns/[category]`, `/submit`,
`/about`), light and dark themes. Authenticated, error, and loading states are excluded because
safe test accounts and fixtures were not supplied.

PASS 1 - Automated (axe-core, agreed public routes, light + dark)
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

No further findings within the agreed coverage. Authenticated, error, and loading states remain
unverified. Awaiting approval to address the next ranked finding or expand coverage.
```

## Why it works
Automated scanners catch missing names and roles but not missing operability, and manual keyboard/tree checks catch the opposite - running only one class of check leaves part of the accessibility surface unverified. Ranking by harm before fixing stops you from burning the session on minor label-wording nits while a critical blocker remains, and re-running the identical checks is strong evidence that the fix did not silently break what already worked.
