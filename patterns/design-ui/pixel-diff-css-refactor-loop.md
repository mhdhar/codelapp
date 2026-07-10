---
title: "The Zero-Pixel Refactor: Screenshot-Diff Loop for CSS Changes"
slug: "pixel-diff-css-refactor-loop"
format: "loop"
category: "design-ui"
tools: ["claude-code", "cursor"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Screenshot every affected route before and after, then loop until the styling refactor changes zero pixels."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["design-token-drift-hunt", "verify-before-claiming-fixed", "responsive-breakpoint-stress-test"]
---

## When to use this
You need to restructure styling code without changing how anything looks: migrating utility classes to tokens, flattening a CSS file, swapping styled-components for CSS modules, deduplicating a component. "It should look identical" is exactly the claim agents get wrong silently. Use this whenever the diff is large but the intended visual change is zero.

## The pattern
```text
I want a styling refactor that changes ZERO pixels. Before you touch any code:

1. Baseline: start the dev server if it isn't running. Trace which routes import the
   files you're about to change, and screenshot every one of them at 375px and 1440px,
   in light and dark theme if the app has both. Save each shot to a scratch folder,
   named route + viewport + theme.
2. Do the refactor.
3. Re-screenshot the exact same routes, viewports, and themes, same filenames plus an
   "after" suffix.
4. Compare every after-shot against its baseline. Use a pixel-diff tool if one is
   available (pixelmatch, odiff, ImageMagick compare); otherwise inspect the pairs side
   by side. Output one line per pair: IDENTICAL or CHANGED, and for CHANGED, what moved
   and by how much.
5. Every CHANGED pair is a regression. Find the cause in your code diff, fix it, and go
   back to step 3. Loop until every pair reads IDENTICAL.

Rules: there are no acceptable visual changes beyond the exceptions I approve. If
you're convinced a change you introduced is actually a bug fix, stop and ask before
keeping it. Anti-aliasing noise under a 0.1% pixel threshold may be ignored, but say
so when you rely on it.

Before you do anything, ask me in one message and wait: (1) what refactor I want,
(2) any visual exceptions I'll accept. If I give no exceptions, there are none.
```

## Real example output
```
Baseline: 4 routes import the files under styles/legacy/ (/, /pricing, /docs, /settings).
16 screenshots taken (4 routes x 2 viewports x 2 themes) in .scratch/baseline/.

Refactor: replaced styles/legacy/buttons.css and cards.css with token-based classes in
styles/components.css, deleted 412 lines, touched 9 component files.

DIFF ROUND 1 (pixelmatch, threshold 0.1):
/           375 light: IDENTICAL   1440 light: IDENTICAL   (dark: both IDENTICAL)
/pricing    375 light: CHANGED, 2,304 px differ. CTA button 2px shorter: legacy class had
            padding: 10px 24px, new token maps to py-2 (8px).
/docs       all 4: IDENTICAL
/settings   1440 dark: CHANGED, 118 px differ. Card border invisible: legacy used
            #2A2A2A, token border-border resolves to #262626 in dark.

Fixes: remapped the button to the py-2.5 token (10px, exact match to legacy). For the
card border I stopped and asked, since the legacy #2A2A2A was off-system and the token
value looks like the intended one. You approved keeping the token. Logged as the only
approved exception.

DIFF ROUND 2: 15 pairs IDENTICAL, 1 approved exception. Refactor lands at -412 lines
with zero unapproved pixel changes.
```

## Why it works
An agent judging its own refactor by re-reading the code will always conclude it's equivalent, because that's what it intended. Screenshots diffed pixel by pixel are evidence from outside the code, and the loop back to step 3 means "fixed" is defined by the diff going quiet, not by the agent's confidence. Capturing the baseline before the first edit is the part you can't reconstruct later.
