---
title: "Dependency Upgrade Breakage Isolation"
slug: "dependency-upgrade-breakage-isolation"
format: "workflow"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Pin down which single dependency bump in a multi-package upgrade actually broke the app."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["git-bisect-regression-hunt", "one-variable-at-a-time", "integration-decision-checklist"]
---

## When to use this
You ran a bulk dependency update (`npm update`, `bundle update`, `pip install -U`, a Dependabot batch PR) touching a dozen packages, and something broke. You don't know which package caused it and don't want to blame the wrong one from a hunch.

## The pattern
```text
A bulk dependency upgrade just broke something.

Do this:

1. Read the lockfile diff yourself (git diff on the lockfile) and list
   every package whose version changed, old -> new. If the lockfile
   change is already committed, diff against the commit before the
   upgrade.
2. Turn what I said broke into a single repro command that exits
   non-zero while broken (a test, a build, a curl check). Show me the
   command. Then revert all the bumps to their old versions and confirm
   the repro command passes at the old versions. This confirms the
   regression is actually in this upgrade and not something else.
3. Reintroduce the version bumps one package at a time, in the order
   they appear in the dependency tree (leaf/low-level packages first,
   since a low-level bump can be the real cause of a higher-level
   package's failure). After each single bump, run the repro command.
4. The moment the repro command fails, stop. That package's version bump
   is the culprit. Do not bump any further packages to "confirm."
5. Look up that package's actual changelog or release notes between the
   old and new version. Quote the specific breaking change that matches
   the observed failure, don't guess which line in the changelog is
   relevant.
6. Report: which package, which version range, which changelog entry,
   and whether the fix should be pinning the old version, upgrading our
   code to match the new API, or filing an upstream issue.

First, ask me what broke after the upgrade and wait for my reply. If
all I can say is "tests fail" or "the build fails," run the test suite
and the build yourself and use the first failure as the symptom.
```

## Real example output
Lockfile diff after `npm update`: 14 packages bumped, including `zod 3.22.4 -> 3.23.0`, `date-fns 2.30.0 -> 3.6.0`, `react-hook-form 7.45.1 -> 7.52.0`.

Step 2: Reverted all 14, ran `npm run build && npm test`, both pass. Confirmed the regression is in this upgrade.

Step 3: Reintroduced bumps in dependency order. After `date-fns` alone: `npm test` fails, 4 tests in `src/utils/__tests__/formatDate.spec.ts` throw `TypeError: (0, date_fns_1.format) is not a function`.

Step 4: Stopped, `date-fns` is the culprit. (`zod` and `react-hook-form` bumps were never reintroduced, no need to test them.)

Step 5: `date-fns` v3 changelog: "v3 is now published as native ESM, CommonJS `require('date-fns')` default import no longer works the same way, use named imports." Our code does `const dateFns = require('date-fns'); dateFns.format(...)`, which matches this breaking change exactly.

Report: `date-fns` v2 -> v3 broke CommonJS default-import usage. Options: pin `date-fns` to `^2.30.0` for now, or migrate the 6 call sites using `require('date-fns')` to named ESM imports (`import { format } from 'date-fns'`). Recommended pinning short-term since the migration touches 6 files across 2 packages.

## Why it works
Bulk upgrades bundle unrelated changes together, so a symptom that "started after npm update" gets blamed on whichever package the developer half-remembers changing, which is often wrong. Reverting everything, confirming the baseline, then reintroducing one change at a time isolates the actual cause instead of the most memorable one, and checking the real changelog turns "this package broke it" into a specific, citable breaking change.
