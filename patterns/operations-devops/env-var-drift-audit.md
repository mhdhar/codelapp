---
title: "Env Var Drift Audit: Code vs Template vs Platform"
slug: "env-var-drift-audit"
format: "goal"
category: "operations-devops"
tools: ["claude-code", "codex-cli"]
difficulty: "advanced"
est_time: "25 min"
models: ["any"]
summary: "Reconcile every env var across code, .env.example, and your deploy platform until nothing is unaccounted for."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
A deploy just failed on a missing env var, or a new machine couldn't boot the project, and you suspect the real inventory of variables lives in three places that quietly disagree: the code, the .env.example, and the deploy platform's dashboard.

## The pattern
```text
Goal: zero unaccounted-for environment variables in this project. A
variable is accounted for when it's consistent across code, the env
template, and the deploy platform, or has a written reason it isn't.

1. Build the inventory from code, which is the source of truth: search
   the codebase for every variable actually read (process.env, os.environ,
   import.meta.env, or this stack's equivalent).
2. Compare against .env.example (or whatever env template this repo uses).
3. Compare against the deploy platform, per environment: list what's set
   using the platform CLI if one is available (e.g. `vercel env ls`).
   If you can't read it from here, print the exact command for me to run
   and work from the output I paste back.
4. Produce a table: variable | read in code | in template | in platform
   (per env) | verdict. Verdicts:
   - OK: present everywhere it needs to be.
   - MISSING: read in code, absent from the platform. Breaks deploys.
   - ORPHANED: set somewhere, never read in code. Delete candidate.
   - UNDOCUMENTED: read in code and set in platform, but absent from the
     template, so a fresh clone can't boot.
5. Fix what needs no secrets: add every UNDOCUMENTED name to the template
   with a placeholder value and a one-line comment. Never write a real
   secret value into any tracked file.
6. For MISSING and ORPHANED rows, output the exact platform command to
   fix each one, but do not run them. Changing live config is my call.

Done means the table has zero MISSING and zero UNDOCUMENTED rows, and
every ORPHANED row has either a removal command or a reason to stay.
```

## Real example output
```
Inventory: 14 variables read in code, 11 in .env.example, 12 set in Vercel.

| Variable              | Code | Template | Prod | Preview | Verdict |
|-----------------------|------|----------|------|---------|---------|
| DATABASE_URL          | yes  | yes      | yes  | yes     | OK |
| STRIPE_WEBHOOK_SECRET | yes  | yes      | yes  | no      | MISSING (preview): webhook tests 500 on preview deploys |
| RESEND_API_KEY        | yes  | no       | yes  | yes     | UNDOCUMENTED -> added to .env.example |
| ADMIN_ALLOWLIST       | yes  | no       | yes  | yes     | UNDOCUMENTED -> added to .env.example |
| NEXT_PUBLIC_GA_ID     | no   | yes      | yes  | yes     | ORPHANED: analytics removed in March, safe to delete |
| ...9 more OK rows...  |      |          |      |         |    |

Template updated: 2 names added with placeholders, no real values written.
Commands for you to run:
  vercel env add STRIPE_WEBHOOK_SECRET preview
  vercel env rm NEXT_PUBLIC_GA_ID production
  vercel env rm NEXT_PUBLIC_GA_ID preview

Remaining: 0 MISSING after you run line 1, 0 UNDOCUMENTED, 1 ORPHANED
with removal commands above.
```

## Why it works
Making the code the source of truth ends the argument about which list is right: a variable nobody reads is dead weight no matter who set it, and a variable the code reads must exist everywhere the code runs. The four verdicts map one-to-one onto real failure modes, so the table is a work order, not a report.
