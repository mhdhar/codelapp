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
---

## When to use this
You built a custom Select, Combobox, Toggle, or Tabs trigger (Base UI, Radix, Headless UI, or your own div-based component) instead of using a native `<select>` or `<button>`. Native elements get an accessible name for free from their own text content; custom ones don't, unless you wire it explicitly. Use this before shipping any form that uses non-native interactive controls.

## The pattern
```text
Audit every custom interactive control in this codebase (or in [FILE OR DIRECTORY]) for a
real accessible name — not just visible text near it, but a name a screen reader announces
when the control receives focus.

For each custom control (Select, Combobox, Listbox, Toggle, Switch, Tabs trigger, custom
Button that wraps a div/span, etc.), check in this order:

1. Does the trigger element itself have visible text content inside it? If yes and that text
   describes its purpose, it likely already has a name — mark OK.
2. If the visible label lives in a separate <label> or text element (not inside the control),
   check whether it is linked via `aria-labelledby` pointing at that label's `id`, or the
   control has an explicit `aria-label`. If neither exists, mark FAIL — this is a WCAG 4.1.2
   "button-name" violation: the control is operable but nameless to assistive tech.
3. If the control shows a placeholder or current value only (e.g. "Select a country"), confirm
   that placeholder text is exposed as the accessible name too, not just rendered as innerText
   that disappears once a value is chosen.
4. Confirm the name updates correctly when the control's state changes (e.g. once a value is
   selected, the accessible name should still make sense, not go blank).

For every FAIL, fix it by linking the existing visible `<Label>` to the trigger with
`aria-labelledby="[LABEL_ID]"` (reuse the label's real id, don't invent a new one), or add
`aria-label="[DESCRIPTIVE NAME]"` if there is no visible label element to reuse.

Output a table: component name, file path, PASS/FAIL, and the fix applied. Do not touch
controls that already pass.
```

## Real example output
```
AUDIT: /submit form custom controls

| Component              | File                                  | Result | Fix |
|-------------------------|---------------------------------------|--------|-----|
| CategorySelect trigger  | components/submit/CategorySelect.tsx  | FAIL | Added aria-labelledby="category-label" pointing at the existing <Label id="category-label"> |
| FormatSelect trigger    | components/submit/FormatSelect.tsx    | FAIL | Added aria-labelledby="format-label" pointing at the existing <Label id="format-label"> |
| DifficultyToggle        | components/submit/DifficultyToggle.tsx | PASS | Visible text "Beginner / Intermediate / Advanced" is inside each toggle option, already named |
| SubmitButton            | components/submit/SubmitButton.tsx    | PASS | Visible text "Submit pattern" inside the button |

Both FAILs share one root cause: Base UI's <Select.Trigger> renders as a button with no text
content of its own — the value is projected into a <Select.Value> span, but before a value is
chosen that span is empty, and the <Label> sitting above it was never connected. Screen
readers announced "combobox, empty" with no indication of what the field was for.

Fix applied to both:
<Label id="category-label">Category</Label>
<Select.Trigger aria-labelledby="category-label">
  <Select.Value placeholder="Choose a category" />
</Select.Trigger>

Re-ran axe-core on /submit after the fix: 0 violations (was 1 critical, 2 instances).
```

## Why it works
Native `<button>` and `<select>` derive their accessible name from their own text content automatically; custom components built from unstyled div or span primitives don't inherit that behavior just because they look and click like the native element. Explicitly linking with `aria-labelledby` is what actually exposes the name to the accessibility tree, not text that happens to sit visually nearby.
