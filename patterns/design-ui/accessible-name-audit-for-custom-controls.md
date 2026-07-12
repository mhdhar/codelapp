---
title: "Accessible Name Audit for Custom UI Controls"
slug: "accessible-name-audit-for-custom-controls"
format: "workflow"
category: "design-ui"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Check every custom select, toggle, and combobox for a real accessible name before you ship it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["highest-impact-a11y-fix-loop", "touch-target-thumb-audit", "component-states-completeness-check"]
---

## When to use this
You built a custom Select, Combobox, Toggle, or Tabs trigger (Base UI, Radix, Headless UI, or your own div-based component) instead of using a native `<select>` or `<button>`. Native controls often expose semantics more predictably, but every control still needs a meaningful computed accessible name. Use this before shipping any form that uses non-native interactive controls.

## The pattern
```text
First ask me for (1) the codebase or directory to inspect and (2) whether I want AUDIT ONLY
(the default) or AUDIT + FIX. Then audit every custom interactive control in that scope for a
real accessible name: the computed name a screen reader announces when the control receives
focus, not just visible text near it.

For each custom control (Select, Combobox, Listbox, Toggle, Switch, Tabs trigger, custom
Button that wraps a div/span, etc.), check in this order:

1. Inspect the browser accessibility tree (or an equivalent computed-accessibility API) and
   record the trigger's computed role and accessible name in its default state. Visible text is
   a clue, not proof. If you cannot inspect the computed result, mark it UNVERIFIED, not PASS.
2. If the visible label lives in a separate `<label>` or text element, check whether the
   control is linked with `aria-labelledby` pointing at that label's `id`, or has a purposeful
   `aria-label`. If neither exists and the computed name is empty or misleading, mark FAIL.
3. If the control shows only a placeholder or current value (for example, "Select a country"),
   confirm the control still has a useful purpose name after a value is selected.
4. Confirm the computed name and role stay correct when the control's state changes.

For every FAIL, propose the smallest fix. In AUDIT ONLY mode, do not edit. In AUDIT + FIX mode,
apply only the confirmed, local fixes: link the existing visible `<Label>` with `aria-labelledby`
when possible, or add a purposeful `aria-label` when no visible label exists. Do not alter
unrelated controls, deploy, or change production data. Re-check the computed name, role, and
keyboard operation after each fix.

Output a table: component name, file path, computed role, computed name, PASS/FAIL/UNVERIFIED,
evidence, and proposed or applied fix. Do not touch controls that already pass.
```

## Real example output
```
AUDIT: /submit form custom controls

| Component              | File                                  | Computed role/name | Result | Fix |
|-------------------------|---------------------------------------|--------------------|--------|-----|
| CategorySelect trigger  | components/submit/CategorySelect.tsx  | combobox / "" | FAIL | Added aria-labelledby="category-label" pointing at the existing <Label id="category-label"> |
| FormatSelect trigger    | components/submit/FormatSelect.tsx    | combobox / "" | FAIL | Added aria-labelledby="format-label" pointing at the existing <Label id="format-label"> |
| DifficultyToggle        | components/submit/DifficultyToggle.tsx | radio / "Beginner" (and each option) | PASS | Computed names match visible options |
| SubmitButton            | components/submit/SubmitButton.tsx    | button / "Submit pattern" | PASS | Computed name matches visible text |

Both FAILs share one root cause: Base UI's <Select.Trigger> renders as a button with no text
content of its own - the value is projected into a <Select.Value> span, but before a value is
chosen that span is empty, and the <Label> sitting above it was never connected. Screen
readers announced "combobox, empty" with no indication of what the field was for.

Fix applied to both:
<Label id="category-label">Category</Label>
<Select.Trigger aria-labelledby="category-label">
  <Select.Value placeholder="Choose a category" />
</Select.Trigger>

Re-ran axe-core and inspected the accessibility tree on /submit after the fix: 0 violations
(was 1 critical, 2 instances); both triggers now have their visible label as the computed name.
```

## Why it works
Accessible names are computed from the control's semantics and its label relationships, not from
visual proximity alone. A custom control that looks labelled can still be announced as empty or
misleading. Inspecting the computed accessibility tree and then re-checking after a minimal fix
tests what assistive technology can actually use.
