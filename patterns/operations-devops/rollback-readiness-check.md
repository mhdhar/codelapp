---
title: "Rollback Readiness Check"
slug: "rollback-readiness-check"
format: "workflow"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Before you deploy, confirm you can actually roll back — not just that the deploy itself is clean."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to deploy something riskier than a copy fix: a schema migration, a new required env var, a dependency bump. A clean build proves the deploy will start; it says nothing about whether you can undo it in five minutes if it goes wrong. Use this before those deploys, not after.

## The pattern
```text
Before I deploy this, check whether I can actually roll it back if it goes
wrong. Do not just check that the deploy is clean, check that undoing it
is possible and fast.

CHANGE BEING DEPLOYED: [WHAT'S SHIPPING — e.g. "new required env var
STRIPE_WEBHOOK_SECRET" or "migration adding a NOT NULL column"]

1. Identify the last known-good deployment (tag, commit hash, or platform
   deployment ID) and confirm it still exists and is redeployable.
2. If this change includes a database migration: state whether it's
   backward-compatible (the old code can still run against the new schema)
   or a breaking change. If breaking, name the specific rollback step
   (a down-migration, a manual SQL fix, or "this cannot be cleanly rolled
   back, only forward-fixed") — don't assume "revert the commit" is enough.
3. If this change adds or renames a required config value (env var, feature
   flag, secret): confirm the OLD code path still works if that value is
   absent, or state plainly that rollback requires restoring the old config
   too, not just the old code.
4. State the actual rollback command or platform action (e.g. "vercel
   rollback [DEPLOYMENT_URL]", "kubectl rollout undo", "revert PR + redeploy")
   and how long it takes in practice, not in theory.
5. Give a one-line verdict: SAFE TO DEPLOY (rollback is fast and complete),
   or ROLLBACK IS PARTIAL (name exactly what wouldn't be undone).
```

## Real example output
```
CHANGE: migration adding a NOT NULL `tenant_id` column to `invoices`, backfilled from `accounts.tenant_id`.

1. Last known-good: deployment dpl_8f2a1c (tagged v1.14.2), still live in Vercel's deployment history, redeployable in one click.

2. Migration is NOT backward-compatible: the old code doesn't set tenant_id on insert, so if we roll back the app code but not the DB, every new invoice insert fails on the NOT NULL constraint. Rollback step: run the down-migration (0042_add_tenant_id_down.sql) BEFORE redeploying old code, not after.

3. No new config values, N/A.

4. Rollback command: `vercel rollback dpl_8f2a1c`, but it must be preceded by the down-migration or the old code will 500 on every invoice write. In practice: down-migration takes ~30s on this table size, then rollback takes ~1 min.

5. VERDICT: ROLLBACK IS PARTIAL — a plain "vercel rollback" alone breaks invoice creation. Full rollback requires the down-migration first. Documented the two-step order above; proceeding with deploy since the sequence is now known and fast.
```

## Why it works
"The deploy is clean" and "I can undo the deploy" are different claims, and conflating them is how teams discover mid-incident that rollback means restoring from a backup instead of clicking one button. Forcing a concrete rollback command and an honest partial/complete verdict turns an assumption into something you actually know before you need it.
