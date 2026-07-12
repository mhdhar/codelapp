---
title: "Pushy Skill Description Audit"
slug: "pushy-skill-description-audit"
format: "loop"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Rewrite thin skill descriptions so agents actually reach for them instead of silently ignoring them."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["reference-repo-to-skill", "meta-descriptions-at-scale"]
---

## When to use this
You've built skills, custom slash commands, or tool descriptions and suspect the agent isn't reaching for them when it should. Use this loop to audit and rewrite every description in a skills directory in one pass.

## The pattern
```text
Find where this project keeps its skill/tool descriptions (.claude/skills/, .cursor/rules/, or equivalent; check the repo), tell me which directory you found, and audit every description in it for missing supported intents or overbroad triggering. If I name a directory in my next message, use that one instead.

For each skill found, loop:
1. Read its current description.
2. Identify the supported user intents and the adjacent requests that must not trigger it.
3. If a common supported intent is missing, or the description would trigger on unsupported work, rewrite it to cover the supported intent precisely and add a short exclusion where useful. Do not optimize for a phrase count.
4. Do not change what the skill does - only rewrite the description/trigger text. Keep it accurate; don't add trigger phrases for things the skill doesn't actually handle.
5. After each rewrite, show the before/after so I can sanity-check it didn't overreach.
6. Move to the next skill. Repeat until every skill in the directory has been checked.
7. At the end, report how many were rewritten vs. already fine.
```

## Real example output
```
Found 6 skills in .claude/skills/.

1. deploy-check/SKILL.md
   Before: "How to check a deployment"
   Supported intent missing: deployment verification and diagnosis.
   After: "Checks deployment health after a deploy. Use when the user asks whether a deployment or release is live, healthy, or failed. Do not use to deploy, publish, or change production."
   Rewritten to add the supported intent and exclude unsupported deployment actions.

2. db-migrate/SKILL.md
   Before: "Runs database migrations. Use when the user mentions migrations, schema changes, altering tables, adding columns, or running db:migrate / prisma migrate / alembic."
   Supported intents are precise and complete. Already fine, no change.

3. api-docs-gen/SKILL.md
   Before: "Generates docs"
   After: "Generates API documentation from route/endpoint code. Use when the user asks to document an API, generate docs, write endpoint documentation, create an OpenAPI/Swagger spec, or says 'document these routes'."
   Rewritten to cover the supported documentation intents.

[... 3 more checked ...]

Summary: 4 of 6 rewritten, 2 already had sufficient trigger coverage.
```

## Why it works
A skill an agent never reaches for is dead weight regardless of how good its contents are, because agents match on the description text, not the quality of what's inside. Covering supported intents while excluding adjacent unsupported work makes a skill findable without causing it to trigger too broadly.
