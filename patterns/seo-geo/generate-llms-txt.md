---
title: "Generate an llms.txt File From Your Site"
slug: "generate-llms-txt"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Build a clean llms.txt index so AI answer engines cite your site accurately."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
aliases: ["llms.txt", "ai crawler index"]
related: ["ai-crawler-robots-txt-strategy", "ai-citation-worthiness-audit", "entity-consistency-audit"]
---

## When to use this
You want ChatGPT, Claude, Perplexity, and other answer engines to cite your site correctly instead of guessing at page content. Use this after your site's information architecture is stable, before a GEO push.

## The pattern
```text
I'm considering an llms.txt file for a named system that I have confirmed
supports it. This is an optional interoperability file, not a general SEO or
AI-visibility tactic. Do not claim it improves Google Search visibility or
that an AI crawler consumes it unless the target system documents that support.

First ask me which system will consume this file and wait. If I cannot name
one, explain that the file is optional and stop rather than generating it by
default.

If you're running inside the site's codebase, build the page list
yourself from the routes and content files, and take the site URL from
the repo's config or sitemap.

Generate an llms.txt file following this spec:
- Start with an H1 with the site or product name
- One blockquote line summarizing what the site/product does, under 40 words
- Group pages into logical H2 sections (e.g. Docs, Product, Company, Legal)
- Under each section, list pages as markdown links: [Page Title](URL): one-line
  description of what an AI would learn from this page
- Keep descriptions factual and specific, no marketing adjectives
- Do not include pages that are duplicate, thin, or behind a login
- End with an "Optional" H2 section for lower-priority pages if any exist

Output the final file as a single markdown code block, ready to save as
/llms.txt at my site root.

If you can't see the codebase, first ask me to paste my key pages (URL
plus a short description each) and the site URL, then wait for my reply.
```

## Real example output
```markdown
# Codel

> Codel is a copy-paste library of AI coding patterns for developers using
> Claude Code, Cursor, Codex CLI, and Gemini CLI.

## Docs
- [Getting Started](https://codel.app/docs/getting-started): how to install and run your first pattern
- [Pattern Format](https://codel.app/docs/format): the frontmatter schema every pattern file follows

## Product
- [Browse Patterns](https://codel.app/patterns): searchable index of all published patterns by category
- [Submit a Pattern](https://codel.app/submit): how to contribute a new pattern for review

## Optional
- [Changelog](https://codel.app/changelog): dated log of site and pattern updates
```

## Why it works
For a system that explicitly supports this convention, a hand-curated plain-text index can provide useful context. It is optional and does not replace crawlable navigation, a sitemap, or foundational SEO, and it should not be presented as a guaranteed citation or ranking mechanism.
