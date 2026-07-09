---
title: "Rubric Grading Loop for AI Output"
slug: "rubric-grading-loop"
format: "workflow"
category: "debugging-evaluation"
tools: ["claude-code"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Score AI-generated code against a checklist and keep iterating until every item passes."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You've finished a feature or PR built with AI help and want a consistent, repeatable quality gate before review, not a vague "looks good to me."

## The pattern
```text
Grade the current changes against this rubric. Use the diff, not memory of
what you intended to write.

Default rubric:
1. All new functions handle invalid or missing input, not just the happy path.
2. No console.log, print statements, or debug leftovers in the diff.
3. Tests exist and pass for every new code path.
4. No hardcoded values (URLs, timeouts, limits) that should be config.

For each item:
- Mark PASS or FAIL.
- If FAIL, quote the exact file and line that violates it and explain why.
- Do not mark PASS unless you can point to the specific code that satisfies it.

After grading, output a scorecard like:
1. PASS/FAIL
2. PASS/FAIL
3. PASS/FAIL
4. PASS/FAIL

If any item is FAIL, fix only that item, then re-run this entire rubric
from item 1 on the updated diff. Repeat until every item is PASS. Do not
skip re-grading items that already passed, a fix can break something else.

Before grading, ask me one question and wait: do I want my own rubric
instead of the default? If I paste criteria (one per line), grade
against those. If I say no or skip it, use the default and start.
```

## Real example output
Scorecard, round 1:
1. FAIL, no error handling in `src/api/upload.ts:22` for missing file param.
2. PASS
3. FAIL, `parseUpload` has no test coverage.
4. PASS

Fixed item 1 and 3. Scorecard, round 2:
1. PASS, added 400 response for missing file param, upload.ts:22-27.
2. PASS
3. PASS, added `parseUpload.test.ts`, 3 cases, all passing.
4. PASS
All items pass. Ready for review.

## Why it works
A named rubric turns "does this look done" into a checklist the AI has to justify line by line, which surfaces the gaps it would otherwise gloss over. Re-grading everything after each fix catches regressions a partial check would miss.
