---
title: "Broken Internal Link Sweep and Fix Loop"
slug: "broken-internal-link-fix-loop"
format: "loop"
category: "seo-geo"
tools: ["claude-code", "codex-cli", "cursor"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Sweep the codebase for internal links that 404 or hit redirects, fix them, re-verify until zero remain."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["internal-linking-gap-analysis", "cta-link-utm-audit-loop", "redirect-map-site-migration"]
---

## When to use this
After a batch of renames, deleted pages, or restructured routes, when you suspect internal links now point at 404s or bounce through redirects. Run it in the actual codebase. It finds link rot at the source instead of waiting for a crawler report weeks later.

## The pattern
```text
Sweep this codebase for broken and chained internal links, then fix them.

1. Build the set of valid paths: enumerate real routes from the router or
   pages/app directory, plus slugs generated from content files, plus any
   redirects config. State how many valid paths you found and where the
   route definitions live.
2. Extract every internal link: href and to props in components, markdown
   links in content files, hardcoded paths in nav or footer config.
3. Compare. Classify each link:
   - BROKEN: resolves to no valid path and no redirect
   - CHAINED: points at the source of a redirect instead of its final
     destination
   - OK: everything else
4. Fix BROKEN links only where the correct target is unambiguous (an
   obvious rename or a moved slug). If two or more plausible targets
   exist, do not pick one. Put it on a list for me with the candidates.
   Fix every CHAINED link by pointing it at the redirect's final target.
5. Re-run steps 2 and 3 from scratch. If anything is still BROKEN or
   CHAINED that you can fix, loop back to step 4.

Stop when a full re-scan shows zero fixable issues. Then report a table:
file and line | old target | new target | classification, followed by the
ambiguous list that needs my call. If the repo has a link checker or a
test suite that covers routes, run it as final confirmation.
```

## Real example output
```
Valid paths: 148 (app/ router: 61 routes, content/blog: 84 slugs,
next.config.js: 3 redirects).
Internal links extracted: 214 across 47 files.

Scan 1: 9 BROKEN, 6 CHAINED. Fixed 7 + 6, looped.
Scan 2: 2 BROKEN remaining, both ambiguous. Zero CHAINED. Done.

file:line                        | old target            | new target                  | class
components/Footer.tsx:38         | /docs/api/v1          | /docs/api                   | BROKEN
content/blog/launch-recap.md:22  | /pricing-old          | /pricing                    | CHAINED
components/Nav.tsx:19            | /features/automation  | /features/workflow-automation | BROKEN
content/blog/q3-update.md:41     | /docs/webhooks        | /docs/api/webhooks          | CHAINED

Needs your call (2):
- app/customers/page.tsx:77 -> /case-studies (candidates:
  /customers/stories, /resources/case-studies)
- content/blog/2024-recap.md:15 -> /roadmap (page deleted; candidates:
  /changelog, or remove the link)

npm run test:links - 0 failures.
```

## Why it works
Internal 404s waste crawl budget and dead-end both users and AI crawlers, while chained links leak signal one hop at a time, and neither shows up until an external crawl weeks later. Rebuilding the valid-path set from the router itself makes the check exact rather than pattern-matched, and the mandatory re-scan after fixing catches the classic failure: a "fix" that points at another moved page.
