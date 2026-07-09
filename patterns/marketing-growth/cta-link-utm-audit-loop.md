---
title: "Hunt Every Broken CTA on Your Marketing Site Until the Report Comes Back Clean"
slug: "cta-link-utm-audit-loop"
format: "loop"
category: "marketing-growth"
tools: ["claude-code", "cursor", "codex-cli"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "A self-verifying loop that finds broken links, dead anchors, and untagged CTAs in your site repo and fixes them."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Your marketing site has grown past the point where anyone knows every page. Campaigns link to anchors that were renamed months ago, half the signup CTAs carry UTM params and half don't, and nobody notices until an attribution report looks wrong. Run this from the site's repo with a coding agent.

## The pattern
```text
Audit every outbound link and CTA in this codebase's marketing pages. First look at the repo structure to find where the marketing pages live (landing pages, pricing, blog templates, nav, footer). Then work in a loop until a full pass reports zero fixable issues.

Each pass:
1. Collect every href and link-like prop in those pages. Build a list: file path, line, URL, link text.
2. Check each link for three failure modes:
   - Broken: internal links pointing at routes or anchors that don't exist in this repo; external links returning 4xx or 5xx (check with curl, HEAD first, retry with GET before calling it dead).
   - Untracked: CTAs pointing at signup, demo, or app pages that are missing the UTM or tracking params the rest of the site uses. Infer the site's own convention from the majority of already-tagged links. Do not invent a new convention.
   - Mismatched: link text promising one destination ("see pricing") while the href goes somewhere else.
3. Fix only what is unambiguous: dead internal anchors where the renamed target is obvious, missing params where the convention is clear, http links where the https version resolves to the same page. For anything ambiguous, like an external 404 with no obvious replacement, do not guess. Put it in a "needs a human" table with your recommended fix.
4. Re-run the full check from step 1 on every file you changed.

Stop only when a complete pass finds zero broken internal links, zero convention-violating CTAs, and every remaining issue sits in the needs-a-human table. Then report: total links checked, every fix as file:line with before and after, and the needs-a-human table.
```

## Real example output
Pass 3 clean. 214 links checked across 31 files.

Fixed (12):
- `app/pricing/page.tsx:88`: `#faq` anchor dead since the FAQ moved; now `/pricing#questions`
- `components/Footer.tsx:41`: `/docs/getting-started` renamed upstream; now `/docs/quickstart`
- `app/blog/post/cta.tsx:19`: signup CTA missing `utm_source=blog&utm_medium=post`, added to match the 9 already-tagged blog CTAs
- 9 more, listed with before/after hrefs.

Needs a human (2):
| Link | Problem | Recommendation |
|---|---|---|
| `partners.tsx:12` -> integration partner's blog post | External 404, no redirect | Link to their docs home or drop the logo card |
| `about.tsx:57` -> press article | Paywalled, returns 403 to curl | Verify manually in a browser |

## Why it works
Link rot and tag drift are grep problems wearing a marketing costume, so a coding agent in the repo beats any external crawler: it sees the routes and anchors that actually exist, not just the rendered pages. The re-check-after-fix loop catches the classic failure where a "fix" points at another dead target, and the needs-a-human table keeps the agent from inventing destinations for genuinely dead external links.
