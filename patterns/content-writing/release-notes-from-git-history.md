---
title: "Release Notes From Git Log, Not From Memory"
slug: "release-notes-from-git-history"
format: "workflow"
category: "content-writing"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "15 min"
models: ["any"]
summary: "Turn the commits since your last release into human release notes, every line tied to a real commit."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["launch-thread-from-shipped-feature", "readme-truth-audit", "engineering-finish-report"]
---

## When to use this
It's release day and nobody remembers what actually shipped. Writing notes from memory misses half the changes and overstates the other half. The real record is already in git.

## The pattern
```text
Step 1: Find the range.
"Find the last release point in this repo: check git tags first, then
release branches or a CHANGELOG heading. If there's genuinely nothing,
default to the last 30 days unless I say otherwise. List every commit
in that range with hash and subject line, and tell me what range you
chose."

Step 2: Draft grouped by user impact.
"Group those commits into release notes with these sections: New,
Improved, Fixed, Breaking. Rules: describe what the user can now do
differently, not what the code does ('CSV export on the reports page',
not 'refactored exporter class'). Merge multiple commits that ship one
feature into one entry. Put pure chores (dependency bumps, CI,
refactors with no behavior change) in a final Internal section I can
delete. Cite the commit hash after every entry."

Step 3: Verify against the diffs, not the messages.
"For each entry, open the cited commit's actual diff and confirm the
entry doesn't promise more than the code delivers. Commit messages
exaggerate. Flag any entry where the diff shows less than the message
claims, and downgrade the wording to match the diff."

Step 4: Read the Breaking section yourself, twice. That's the one
users screenshot when it's wrong.
```

## Real example output
Range: v1.8.0..HEAD, 47 commits.

**New**
- Export any report as CSV from the reports page (a3f91c2, 8bd0e11)
- Dark mode follows your OS setting automatically (5c2a9f0)

**Fixed**
- Webhook retries no longer fire twice when a delivery times out (e77d431)

**Breaking**
- `GET /v1/reports` now paginates at 100 rows; clients reading the raw array must follow `next_cursor` (91cc7a8)

Step 3 flagged one entry: commit f20b115 says "add SSO support" but the diff only adds the config schema, no login flow. Moved from New to Internal.

## Why it works
Git history is the one source that can't forget what shipped, and self-discovery of the range means there's nothing to hand the agent before it starts. The diff check in step 3 catches the classic failure: release notes written from commit messages inherit every exaggeration in them. Hash citations make each claim auditable in one click.
