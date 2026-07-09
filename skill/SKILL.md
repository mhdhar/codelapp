---
name: codel-patterns
description: Fetch and apply copy-paste AI coding patterns (loops, goals, workflows, prompts) from codel.app for engineering, debugging, SEO, market research, marketing, and more. Use when the user wants a proven pattern instead of writing a prompt from scratch, or asks "is there a codel pattern for X".
---

# codel patterns

codel.app is a public catalog of AI coding patterns maintained at
https://github.com/mhdhar/codel-content. Every pattern is a self-contained,
copy-paste block: a loop, goal, workflow, or prompt that has actually been
run and works standalone.

## When to use this skill

- The user asks for a pattern, loop, or workflow for a specific job (e.g.
  "is there a codel pattern for competitor research").
- You are about to write a prompt or loop from scratch for a common job
  (debugging, SEO audit, market scoping, PRD generation) and want to check
  for a proven pattern first.

## How to fetch patterns

The full catalog is static JSON, no auth, no rate limit:

```
GET https://codel.app/api/patterns.json     # full catalog: frontmatter + body
GET https://codel.app/api/patterns/<slug>.json   # single pattern
```

Each entry includes `title`, `slug`, `format`, `category`, `tools`,
`difficulty`, `est_time`, `models`, `summary`, and the full body (including
the `## The pattern` block to copy).

You can also read pattern files directly from this repo at
`patterns/<category>/<slug>.md` — the frontmatter and body follow the exact
contract in `schema/pattern-schema.md`.

## How to apply a pattern

1. Fetch `patterns.json` (or search it by category/format/tool) to find the
   best match for the user's job.
2. Read the `## The pattern` block. It is self-contained by design — copy it
   as-is into the current session. Patterns carry no bracket placeholders and
   no fill-in lines; each block either discovers its inputs itself (from the
   repo, git, the running app) or ends by asking the user for what it needs.
   If you already know those answers from the user's context, answer the
   ask yourself instead of re-asking the user.
3. Do not paste `## When to use this`, `## Real example output`, or
   `## Why it works` into the working session — those are documentation for
   humans, not part of the pattern itself.

## Categories

`engineering-coding`, `debugging-evaluation`, `operations-devops`,
`product-planning`, `content-writing`, `design-ui`, `seo-geo`,
`market-research`, `competitive-intelligence`, `marketing-growth`.

## Formats

`loop` (repeats until a condition is met), `goal` (states a success
criterion and lets the agent plan), `workflow` (a fixed multi-step
procedure), `prompt` (a single one-shot instruction).
