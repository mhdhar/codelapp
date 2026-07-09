---
title: "Leaf-to-Root Build Order"
slug: "leaf-to-root-build-order"
format: "workflow"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Build UI leaf-first: components, then composites, then pages, routing, state, and API."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're building a new screen or feature from scratch and the codebase has no existing pattern to copy. Use this build order instead of starting from the page or the API, which is where most half-finished, hard-to-review PRs come from.

## The pattern
```text
Build the feature or screen named at the end of this message using this order. Do not skip ahead to later stages before earlier ones are done and reviewed.

STAGE 1 — LEAF COMPONENTS: Identify the smallest, purely presentational pieces this screen needs (e.g. a badge, a stat card, an input with validation state). Build each with no data fetching and no business logic — hardcoded/mock props only. Show me each one before moving on.

STAGE 2 — COMPOSITE COMPONENTS: Combine leaves into the larger sections of the screen (e.g. a filter bar, a results table). Still mock data, no real fetching.

STAGE 3 — PAGE: Assemble the composites into the actual page layout. Still mock data.

STAGE 4 — ROUTING: Wire up the URL/route for the page, including any route params it needs.

STAGE 5 — STATE: Replace mock data with real local/client state (loading, error, empty states included).

STAGE 6 — API: Wire STATE to the real backend endpoint(s). Verify against a real request, not mock data, before calling this done.

After each stage, tell me what you built and wait for a go-ahead before starting the next stage.

The feature or screen to build:
```

## Real example output
```
STAGE 1 — LEAF COMPONENTS: built <StatusBadge status="active|paused|expired" /> and <RenewalCountdown days={14} /> — both pure, hardcoded props, no imports beyond React. Screenshot attached.
> looks good, continue

STAGE 2 — COMPOSITE: built <ContractRow /> combining both leaves plus vendor name/date columns, and <ContractFilterBar /> (status dropdown + date range). Rendered with 5 hardcoded mock contracts.
> continue

STAGE 3 — PAGE: assembled <ContractsPage /> — filter bar on top, table of ContractRow below, still mock data.
> continue

STAGE 4 — ROUTING: added /contracts route in app/contracts/page.tsx, no params needed (filters are client-side query state).
> continue

STAGE 5 — STATE: replaced mock array with useState + loading/error/empty states. Empty state shows "No contracts match these filters."
> continue

STAGE 6 — API: wired to GET /api/contracts?status=&from=&to=. Verified against real dev DB — 38 contracts returned unfiltered, filtering by status=expired correctly returned 6. Build done.
```

## Why it works
Starting from the API or the page tends to produce a screen that "sort of works" everywhere at once and is hard to review in pieces. Building leaf-first gives you a checkable artifact at every stage, so by the time real data gets wired in at stage 6, any failure is a real backend issue, not UI guesswork.
