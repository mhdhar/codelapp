---
title: "Repomix Context Handoff"
slug: "repomix-context-handoff"
format: "workflow"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Pack the full codebase into one file before closing a session, so the next agent inherits real code."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're closing a session on a project the next session, possibly a different agent, possibly days later, needs to resume with full accuracy. Use it alongside a written handoff note, not instead of one: the note explains the plan, this gives the next agent the actual code.

## The pattern
```text
Before we close this session, pack the codebase for the next session.

1. Run: npx repomix
2. Confirm it produced an output file (repomix-output.xml or .md) and tell me its size and how many files it packed.
3. If the repo has a .repomixignore or the default ignore misses something huge and irrelevant (node_modules, build output, lockfiles), tell me and re-run with the exclusion.
4. Write a short HANDOFF.md (if one doesn't exist) or update the existing one with:
   - Current task and status
   - Reference to the repomix output file by name, with the instruction "read this first for full code context"
5. Confirm both files are saved in the project root and tell me whether to commit them.
```

## Real example output
```
1. Ran `npx repomix`. Output: repomix-output.xml, 312 files packed, 1.4MB (tree-sitter compressed from an estimated 4.8MB raw).

2. Default ignore already excludes node_modules and .next/, but it packed 40 files under /coverage (test coverage HTML reports, irrelevant to code context). Re-ran with `npx repomix --ignore "coverage/**"` — now 289 files, 980KB.

3. Updated HANDOFF.md:
   Task: migrate legacy /api/v1/* routes to /api/v2 with the new response envelope.
   Status: 6 of 14 routes migrated (orders, users, invoices, payments, refunds, webhooks). Remaining: reports, exports, admin/*, integrations/*.
   Read repomix-output.xml first for full current code state — this note is the plan, that file is the ground truth.

4. Both files saved at project root. Recommend committing both — repomix-output.xml is 980KB, under the repo's usual diff size, and having it versioned means any session can regenerate a diff against the last known-good pack.
```

## Why it works
A handoff note tells the next session what happened; it can't tell it what the code actually looks like right now, and summaries drift from reality fast. A compressed full-repo pack means the next session starts from real, current code instead of reconstructing it file by file through exploration.
