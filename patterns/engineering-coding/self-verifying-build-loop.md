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
related: ["task-to-self-verifying-brief", "ban-should-work-now-claims", "core-engineering-loop"]
---

## When to use this
You're asking an agent to write or change code and you don't want it to declare victory after a change that merely "looks right." Use this whenever there's a build, test suite, or reproducible failing case to check against.

## The pattern
```text
Implement the change I give you and do not stop until you have real passing evidence.

First, pick the verify command: the build, test, or repro command that proves this change works. Look in package.json scripts, the Makefile, or CI config. State the command you picked and why. If I name a command, use mine instead.

Loop:
1. Write or edit the code for the change.
2. Run the verify command. Record the command, environment, exit status, and
   relevant redacted output, not a summary or sensitive full log.
3. If it fails: identify the root cause from the actual error message (not a guess). State the root cause in one sentence before touching code again.
4. Fix only what the root cause requires. Do not make unrelated changes.
5. Re-run the verify command.
6. Repeat steps 3-5 until the command passes cleanly, or until you've tried 5 times, in which case stop and report what you tried and what's still failing.

Never report the change as done without a final redacted passing record for the verify command. "Looks correct" is not evidence.

Before anything else, ask me in one message and wait: the change to implement or fix, plus a verify command if I have one in mind.
```

## Real example output
```
Verify command: npm run build (runs tsc --noEmit then vite build per package.json; no test covers this path yet).

Attempt 1: npm run build fails.
  TS2345: Argument of type 'string | undefined' is not assignable to type 'string' in src/lib/parseConfig.ts:42
  Root cause: config.env.API_KEY can be undefined at build time; function signature assumes it's always defined.
  Fix: added a guard that throws a clear error if API_KEY is missing, instead of loosening the type.

Attempt 2: npm run build (local environment, exit 0)
  > tsc --noEmit
  > vite build
  ✓ 412 modules transformed.
  build finished in 3.1s
  PASS

Reporting done: build is green, output above. No unrelated files touched.
```

## Why it works
Agents left unchecked will often patch the symptom (silence a type error, add a try/catch) instead of the cause. Requiring a one-sentence root cause before each fix, plus pasted real output, blocks both premature "done" claims and cosmetic patches that don't actually fix anything.
