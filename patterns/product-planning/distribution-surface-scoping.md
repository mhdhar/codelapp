---
title: "Decide the Distribution Surface Before You Scope the Build"
slug: "distribution-surface-scoping"
format: "goal"
category: "product-planning"
tools: ["universal"]
difficulty: "advanced"
est_time: "15 min"
models: ["any"]
summary: "Decide if this ships an API or MCP surface before you scope it — that decision changes the build."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to write a spec or start scoping a project that has any kind of API, data, or service behind it. Deciding "should other apps or AI agents be able to call this" after the build is mostly done means retrofitting auth, versioning, and docs. Decide it now, before a single feature is scoped.

## The pattern
```text
Before scoping [PROJECT DESCRIPTION], decide its distribution surface.

1. Will anything outside the primary UI need to call this programmatically in the next 2 releases — another app, a script, or an AI agent/MCP client? Answer yes/no with your reasoning, don't default to "maybe, just in case."
2. If yes: does an MCP server make sense as the distribution channel (i.e., would an AI agent plausibly want to call this directly), or is a plain REST/webhook API enough? Justify in one sentence.
3. If yes: list what this adds to scope right now, before a single feature is built:
   - Auth model for machine callers (API keys, OAuth, service tokens) separate from human login
   - Versioning strategy for the API contract
   - Rate limiting / abuse handling for non-UI callers
   - Docs format (OpenAPI spec, MCP tool schema, or both)
4. If no: state explicitly that the internal API stays private and unversioned for now, and name the trigger condition that would force a revisit (e.g., "revisit if a partner asks to integrate").

Output as a short decision memo, under 200 words.
```

## Real example output
"1. Yes — a bookkeeping SaaS partner has already asked about pulling categorized expense data programmatically, and freelancers using AI tax-prep assistants are a named target user.
2. MCP server, not just REST. The target caller is an AI agent doing tax prep on the user's behalf, not a human-run script — an MCP tool schema is the more natural fit than raw REST docs.
3. Scope added now: API-key-based auth for machine callers (separate from session auth), a `v1` prefix on every public endpoint from day one, per-key rate limits (100 req/day free tier), and an MCP tool schema alongside the REST docs.
4. N/A — distribution surface is committed for launch, not deferred.

Decision: build the MCP server as a companion package from week one, not bolted on later. Adds roughly 3 days to the timeline, avoids an auth and versioning retrofit later."

## Why it works
Naming auth, versioning, and rate limiting as scope items at decision time — not implementation time — means they get estimated and prioritized like any other requirement, instead of surfacing as a surprise rewrite the day the first external caller shows up.
