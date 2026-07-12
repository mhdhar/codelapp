---
title: "Pre-Deploy Safety Check"
slug: "pre-deploy-safety-check"
format: "workflow"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Confirm build, routes, and diff are clean before you push to production."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["rollback-readiness-check", "env-var-drift-audit", "pre-push-merge-approval-gate"]
---

## When to use this
You're about to deploy to production and want a real check, not a guess. Something builds locally, but you haven't confirmed the diff is secret-free or that the routes people actually use still respond.

## The pattern
```text
Before I deploy to production, run this pre-deploy check and report pass/fail
for each step. First confirm the exact target environment and the currently
deployed commit or deployment ID. Stop and tell me if anything fails; this
check does not authorize a deployment.

1. Confirm the working branch and that `git status` contains only intended
   deploy changes. Do not require unrelated work to be cleaned up.
2. Diff the confirmed deployed commit or deployment ID to HEAD, then scan it for
   hardcoded API keys, tokens, passwords, or connection strings. List any
   file where you find one.
3. Run the project's type check and build commands. Report errors verbatim,
   don't summarize them away.
4. Run the test suite. Report pass/fail counts.
5. List the app's critical routes or health check endpoints from the routing
   config. If a dev/preview server is available, hit each one and report the
   status code. Also report any production config, migration, or secret change
   that needs environment-specific confirmation before deployment.
6. Give me a final verdict: READY FOR APPROVED DEPLOY or DO NOT DEPLOY, with
   the specific reason if it's a no.
```

## Real example output
```
Target: production, deployed commit: 8c24f01. Branch: main; git status contains only intended deploy changes.

git diff 8c24f01...HEAD: no secrets found. One flagged line reviewed and
confirmed to be a placeholder (STRIPE_KEY=sk_test_placeholder in .env.example),
not a real key.

Typecheck: tsc --noEmit -> 0 errors.
Build: next build -> succeeded, no warnings.
Tests: 118 passed, 0 failed.

Routes checked against local preview:
  / -> 200
  /api/health -> 200
  /dashboard -> 200
  /api/orders/export -> 200
  /pricing -> 200

Production config and migration changes: none.

Verdict: READY FOR APPROVED DEPLOY.
```

## Why it works
Each check catches a different failure class: the diff scan catches leaked secrets, the build/test steps catch code that's broken but not obviously so, and the route sweep catches config or routing regressions a green build won't show. Running all three together is what "verified" actually means, not just "it compiled."
