---
title: "CI Failure Triage Loop: Code, Flake, or Infra"
slug: "ci-failure-triage-loop"
format: "loop"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Classify every red CI run as code, flake, or infra, then fix or retry with proof until the branch is green."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["flaky-test-triage-loop", "git-bisect-regression-hunt", "pre-deploy-safety-check"]
---

## When to use this
CI is red on your branch and the tempting move is to smash re-run until it goes green. That hides real bugs and wastes an hour. Run this instead: every failure gets classified with evidence, and re-run is only allowed when the evidence says flake.

## The pattern
```text
CI is red on this branch. Get it green with evidence, not by hammering
re-run. Loop until the latest full run passes:

1. Find the CI provider from the repo config (.github/workflows,
   .gitlab-ci.yml, or similar) and pull the logs of the most recent
   failed run (e.g. `gh run list` then `gh run view --log-failed`).
2. Classify each failing job as exactly one of:
   - CODE: deterministic failure caused by this branch's changes.
   - FLAKE: timing or ordering failure. Before claiming this, check
     whether the same test failed recently on the default branch or on
     an unrelated run. No prior history means no flake claim.
   - INFRA: runner death, network timeout, quota, or dependency download
     failure unrelated to the diff.
3. Act by class:
   - CODE: reproduce locally with the same command CI ran. Fix it, re-run
     that command locally until it passes, then push. Never push a fix
     you haven't run locally.
   - FLAKE: re-run the job once. If it fails again the same way, it was
     not a flake. Reclassify as CODE and say the first call was wrong.
   - INFRA: re-run once. If it recurs, quote the exact infra error and
     stop. Never retry any single failure more than twice.
4. After each push or re-run, wait for the new result and go back to
   step 1.

Exit only when the run is fully green. Then report a table:
run ID | failing job | class | action taken | result.
If anything hits the retry limit, escalate it to me with the verbatim
error instead of retrying silently.
```

## Real example output
```
| Run    | Failing job        | Class | Action                                    | Result |
|--------|--------------------|-------|-------------------------------------------|--------|
| #4821  | test (unit)        | CODE  | Repro'd locally: date mock missing tz. Fixed orders.test.ts, local suite green, pushed 9f2c1ea | fixed  |
| #4822  | test (e2e)         | FLAKE | checkout.spec.ts timeout also failed on main run #4790. Re-ran once | green  |
| #4822  | build (arm64)      | INFRA | Runner lost connection at cache restore. Re-ran once | green  |
| #4823  | full pipeline      | -     | -                                         | GREEN  |

3 runs, 1 real bug, 1 confirmed flake (linked to existing issue #212),
1 infra blip. Branch is green at 9f2c1ea.
```

## Why it works
The classification forces a falsifiable claim before any retry: "flake" requires prior history, and a second identical failure automatically converts it to a real bug. The two-retry ceiling and local reproduction rule are what separate this from the re-run roulette it replaces.
