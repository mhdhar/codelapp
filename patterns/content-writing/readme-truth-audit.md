---
title: "Make the README Stop Lying"
slug: "readme-truth-audit"
format: "goal"
category: "content-writing"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Verify every command, path, and claim in your README against the actual repo, then fix what drifted."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
The code moved on and the README didn't. New contributors follow the install steps and hit errors, or the feature list describes the project as it was two quarters ago. You want the README verified against reality, not skimmed and reworded.

## The pattern
```text
Goal: every checkable claim in this repo's README is true. Zero claims
that describe a version of the project that no longer exists.

Process:
1. Read the README and list every checkable claim: install and run
   commands, CLI flags, env vars, file paths, config keys, feature
   descriptions, supported versions, and links to files in this repo.
2. Verify each one against the repo itself:
   - Commands: run them. Use a dry-run or a temp directory if a command
     is destructive. They must exit cleanly.
   - Paths, flags, env vars, config keys: confirm they exist in the
     code, not in your assumption of what projects usually look like.
   - Feature claims: find the code that implements each one. If you
     can't find it, the claim is stale.
3. Fix the README in place. Match its existing tone and formatting.
   If a claim is aspirational (a planned feature), don't delete it,
   move it under a clearly labeled "Planned" line.
4. Anything you can't verify either way (needs credentials, external
   service, hardware), collect under "Couldn't verify" with what you'd
   need from me.
5. Done when: every command has actually been executed successfully,
   every path and flag is confirmed in code, and the "Couldn't verify"
   list is the only thing left unchecked.

Finish with a table: claim, status (verified / fixed / couldn't
verify), evidence (command output or file:line).
```

## Real example output
| Claim | Status | Evidence |
|---|---|---|
| `npm run dev` starts on port 3000 | fixed | dev server binds 5173 in `vite.config.ts:8`; README now says 5173 |
| `--config` flag accepts a path | verified | `src/cli.ts:42`, ran `node dist/cli.js --config ./fixtures/a.toml`, exit 0 |
| "Supports Postgres and MySQL" | fixed | only `pg` in dependencies, no MySQL driver anywhere; MySQL moved under Planned |
| `docker compose up` boots the stack | couldn't verify | needs Docker daemon, not available here |

Install section: 2 of 5 commands failed as written (`pnpm` vs documented `npm`). Both fixed and re-run clean.

## Why it works
"Run the command" is a stronger check than "does this look right," and it's the difference between an audit and a paraphrase. The explicit done-condition stops the agent from declaring victory after fixing the two most obvious problems, and the evidence column makes every fix spot-checkable in seconds.
