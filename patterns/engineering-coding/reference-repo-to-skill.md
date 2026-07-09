---
title: "Reference Repo to Reusable Skill"
slug: "reference-repo-to-skill"
format: "workflow"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Study a reference codebase once, then extract its patterns into a reusable skill instead of re-reading it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You've found a well-built reference codebase in a domain you're about to build in (a similar app, a library's example repo, a past project of your own) and expect to write more code in that domain again. Use this instead of skimming the reference once and forgetting what you learned.

## The pattern
```text
I want to turn this reference codebase into a reusable skill, not just borrow one snippet from it.

REFERENCE REPO: [PATH OR URL]
DOMAIN: [WHAT THIS REPO IS AN EXAMPLE OF, E.G. "multi-tenant billing" OR "offline-first sync"]

1. Read through the reference repo's structure and identify the 3-6 patterns that are actually reusable (not project-specific naming or business logic) — architectural choices, data modeling approaches, error-handling conventions, folder structure.
2. For each pattern, write: what it is, why it's done that way (infer from the code, don't guess wildly), and the minimal code shape someone would copy.
3. Generate a skill file (SKILL.md or equivalent) that captures these patterns, written so a fresh agent with no memory of this reference repo could apply them correctly.
4. Save it to [SKILLS DIRECTORY, E.G. .claude/skills/ OR ~/.claude/skills/].
5. Tell me which patterns you deliberately left out and why (e.g. too project-specific, tied to a proprietary service).
```

## Real example output
```
REFERENCE REPO: ~/code/old-projects/fleet-tracker
DOMAIN: real-time location tracking with offline queue

Patterns extracted:
1. Offline-first write queue — writes go to a local IndexedDB table first, tagged with a client-generated UUID and synced status, then a background sync worker retries against the API. Reusable shape: writeQueue table schema + sync worker loop.
2. Optimistic UI with rollback — UI updates immediately from the local write, then reconciles or rolls back based on the sync worker's result. Reusable shape: the reducer pattern separating "optimistic state" from "confirmed state."
3. Conflict resolution via last-write-wins + server timestamp — simple but explicit, avoids silent data loss. Reusable shape: the timestamp comparison function.

Left out: the MapBox integration code (tied to a specific paid API key and this project's map styling) and the vehicle-specific data model (fields like fuel_level don't generalize).

Generated skill saved to .claude/skills/offline-first-sync/SKILL.md — covers patterns 1-3 above with the reusable code shapes, written generically (no MapBox, no vehicle fields) so it applies to any offline-write-queue scenario, not just fleet tracking.
```

## Why it works
Reading a good reference repo once teaches you something for an hour; converting it into a skill file teaches every future agent session something, permanently, without re-reading the reference. Listing what got left out is what keeps the extracted skill actually reusable instead of secretly still coupled to the original project.
