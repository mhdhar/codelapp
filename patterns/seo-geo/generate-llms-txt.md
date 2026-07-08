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
---

## When to use this
You want ChatGPT, Claude, Perplexity, and other answer engines to cite your site correctly instead of guessing at page content. Use this after your site's information architecture is stable, before a GEO push.

## The pattern
```text
I'm building an llms.txt file for my site at [SITE_URL]. This is a markdown file
that gives AI crawlers a clean index of the site's most important pages.

Here is a list of my key pages with their URLs and a short description of what
each one covers:
[PAGE_LIST]

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
Answer engines can't reliably parse JS-heavy nav or infer page importance from a sitemap alone. A hand-curated, plain-text index with one-line summaries gives them exactly the context they need to quote your site correctly instead of hallucinating what a page says.
