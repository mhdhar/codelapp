---
title: "Service Provisioning Decision Pass"
slug: "service-provisioning-decision-pass"
format: "goal"
category: "operations-devops"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Decide every third-party service a new build needs before wiring any of them ad hoc."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're starting a new build (or untangling a stalled one) that will eventually touch auth, payments, storage, deploy, and monitoring. You want one deliberate decision per category up front, instead of bolting on whatever service a feature happens to need that week.

## The pattern
```text
Goal: produce a Service Registry for [PROJECT_NAME] — one deliberate
decision per category below, not services added ad hoc as features come up.

For each category, either name the service and justify it in one sentence,
or mark "not needed for this build" and say why:
- Auth
- Payments (if this product ever charges money)
- Database / persistence
- File / media storage
- Email delivery
- Deploy target
- Error monitoring
- Background jobs / queue (if any async work exists)

Rules:
- Prefer a service with an official Claude Code / agent skill or MCP server
  over one without, all else equal — it means future sessions can operate
  it directly instead of you hand-holding through a dashboard.
- Do not pick two services that do the same job "to be safe." One choice
  per category.
- Flag any category where the free tier will not survive expected launch
  traffic, and say what the paid tier costs.
- Output as a table: category | service chosen | why | install/setup command.
- Stop after producing the table. Do not start installing anything —
  this is a decision pass, not implementation.
```

## Real example output
```
Service Registry — SilkBridge (MENA-China trade intelligence tool)

| Category | Service | Why | Setup |
|----------|---------|-----|-------|
| Auth | Clerk | Needs org-level accounts (multiple analysts per client); Clerk's org model fits without custom RBAC | npm i @clerk/nextjs |
| Payments | Not needed for this build | Sold via manual invoicing to enterprise clients, no in-app billing planned | — |
| Database | Supabase (Postgres) | Relational joins across companies/shipments/tariffs; official Claude skill for schema work | npx supabase init |
| File storage | Supabase Storage | Already paying for Supabase, avoids a second storage bill for PDF report exports | included |
| Email delivery | Not needed for this build | Reports delivered in-app only, no transactional email in v1 | — |
| Deploy | Vercel | Official skill for deploy/DNS/edge, team already on it for 3 other projects | npx vercel |
| Error monitoring | Sentry | Official install wizard, needed before the first client demo, not after | npx @sentry/wizard |
| Background jobs | Vercel Cron | Tariff-scrape job runs once daily, doesn't need a full queue system yet | vercel.json cron config |

Flag: Supabase free tier caps at 500MB DB / 1GB storage — tariff history
data will exceed that within roughly 2 months at expected ingest rate.
Pro tier is $25/mo, budget for it before month 2.
```

## Why it works
Deciding every category in one pass surfaces conflicts (two payment processors, no error monitoring until production breaks) before they're buried in separate feature branches. Preferring services with an official agent skill is a compounding choice — every future session can operate that service directly instead of you walking it through a dashboard each time.
