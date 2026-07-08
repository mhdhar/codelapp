---
title: "Self-Verifying Build Loop"
slug: "self-verifying-build-loop"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Make the agent build, run, diagnose, and retry until there's real passing evidence, not a hunch."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You're asking an agent to write or change code and you don't want it to declare victory after a change that merely "looks right." Use this whenever there's a build, test suite, or reproducible failing case to check against.

## The pattern
```text
Implement this change and do not stop until you have real passing evidence.

CHANGE: [WHAT TO IMPLEMENT OR FIX]
BUILD/TEST COMMAND: [THE COMMAND THAT PROVES IT WORKS]

Loop:
1. Write or edit the code for the change.
2. Run BUILD/TEST COMMAND. Paste the actual output, not a summary.
3. If it fails: identify the root cause from the actual error message (not a guess). State the root cause in one sentence before touching code again.
4. Fix only what the root cause requires. Do not make unrelated changes.
5. Re-run BUILD/TEST COMMAND.
6. Repeat steps 3-5 until the command passes cleanly, or until you've tried 5 times, in which case stop and report what you tried and what's still failing.

Never report the change as done without pasting the final passing output. "Looks correct" is not evidence.
```

## Real example output
```
Attempt 1: npm run build fails.
  TS2345: Argument of type 'string | undefined' is not assignable to type 'string' in src/lib/parseConfig.ts:42
  Root cause: config.env.API_KEY can be undefined at build time; function signature assumes it's always defined.
  Fix: added a guard that throws a clear error if API_KEY is missing, instead of loosening the type.

Attempt 2: npm run build
  > tsc --noEmit
  > vite build
  ✓ 412 modules transformed.
  build finished in 3.1s
  PASS

Reporting done: build is green, output above. No unrelated files touched.
```

## Why it works
Agents left unchecked will often patch the symptom (silence a type error, add a try/catch) instead of the cause. Requiring a one-sentence root cause before each fix, plus pasted real output, blocks both premature "done" claims and cosmetic patches that don't actually fix anything.
