---
title: "Generate Meta Descriptions at Scale Across Many Pages"
slug: "meta-descriptions-at-scale"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Batch-write unique, click-worthy meta descriptions for dozens of pages at once."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
A site audit turns up dozens or hundreds of pages missing meta descriptions, or with the same template-generated line copy-pasted across every one. Use this when you have a page list with title and a one-line summary per page and need real descriptions fast.

## The pattern
```text
Below the line is a list of pages on my site that need meta descriptions.
Each row has: URL, page title, and a one-line summary of what the page
covers.

Write one meta description per page:
- 140-155 characters, hard cap 160
- Includes the primary topic/keyword from the title naturally, not stuffed
- States a concrete reason to click: a number, an outcome, or what the
  reader will be able to do — not generic phrasing like "learn more about X"
- No two descriptions in the output may be near-duplicates of each other,
  even if the source pages are similar (e.g. two pricing-tier pages need
  descriptions that differ by what's actually different about that tier)
- No surrounding quotation marks, no title-casing every word, write it like
  a real sentence

Output as a table: URL | Character count | Meta description. Flag any row
where the character count exceeds 160 so I can trim it.

Paste the page list below this line:
```

## Real example output
```
URL                                  | Chars | Meta description
/pricing/starter                     | 148   | Starter plan: 3 seats, 500 tasks/mo, and core automations for $19/mo. No credit card needed for the 14-day trial.
/pricing/team                        | 152   | Team plan: unlimited seats, 5,000 tasks/mo, and shared workflows for $49/mo. Includes priority email support.
/pricing/enterprise                  | 143   | Enterprise pricing scales with usage: SSO, audit logs, and a dedicated account manager. Talk to sales for a quote.
/blog/reduce-invoice-errors          | 151   | Cut invoice errors by automating line-item matching. A 6-step checklist teams use to catch mistakes before send.
/docs/api/webhooks                   | 165   | FLAG - trim: Set up webhooks to receive real-time events for task creation, updates, completions, and deletions.
```

## Why it works
Writing descriptions one page at a time drifts into the same three or four templates by page 20. Forcing every row through the same constraint list — and an explicit non-duplication rule across the whole batch — catches the copy-paste failure mode that a per-page prompt never surfaces.
