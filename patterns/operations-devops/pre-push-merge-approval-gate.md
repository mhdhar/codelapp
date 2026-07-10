---
title: "Pre-Push, Pre-Merge Approval Gate"
slug: "pre-push-merge-approval-gate"
format: "workflow"
category: "operations-devops"
tools: ["claude-code", "codex-cli", "cursor"]
difficulty: "intermediate"
est_time: "8 min"
models: ["any"]
summary: "Stop an agent from pushing or merging without your explicit go-ahead, every time."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["ui-approval-gate-checkpoints", "pre-deploy-safety-check", "hostile-reviewer-pass"]
---

## When to use this
You run agent sessions across several branches at once, with local and experimental work in flight. Local edits should stay free-flowing, but anything that leaves your machine (a push, a merge, a PR merge) needs you to say yes first, not an agent's judgment call.

## The pattern
```text
Before you run `git push`, `git merge`, `gh pr merge`, or any command that
moves this branch's history onto a shared or remote target, stop and do the
following instead of running it:

1. State exactly what command you're about to run and its target
   (branch name, remote, or PR number).
2. Summarize what's included: number of commits, files touched, and whether
   this branch has any known-incomplete or experimental work mixed in.
3. Confirm local state is what you think it is: run `git status` and
   `git log --oneline` from the branch this merges into (the repo's
   default branch unless this branch targets another) up to HEAD, and
   show me both outputs.
4. Ask a direct yes/no question that names the exact action and target,
   like: "OK to push feature/rate-limit-headers to origin? (yes/no)"
5. Do not run the push/merge command until I reply with an explicit yes.
   A message that only approves a different step, or says nothing about
   this specific action, is not approval.

This gate applies only to push, merge, and anything that publishes history.
Local commits, local branch creation, stashing, and running tests are fine
to do without asking; don't stop for those.
```

## Real example output
```
About to run: git push origin feature/rate-limit-headers

Summary: 3 commits, 4 files (src/middleware/rateLimit.ts,
src/middleware/rateLimit.test.ts, src/config/limits.ts, CHANGELOG.md).
No experimental code mixed in: the WIP retry-logic commit was left on a
separate stash, not included here.

git status: working tree clean.
git log main..HEAD --oneline:
  a3f1c9d add rate limit response headers
  8e2b410 add config for per-route limits
  1d9a044 test: rate limit header assertions

OK to push feature/rate-limit-headers to origin? (yes/no)

> yes

Pushed. origin/feature/rate-limit-headers now up to date.
```

## Why it works
Naming the exact command and target before running it turns "the agent decided this was fine" into "you confirmed this specific action." Requiring a direct yes tied to that specific ask closes the loophole where an agent treats a vague "sounds good" from three messages ago as blanket approval.
