---
title: "Write a robots.txt Strategy for AI Crawlers"
slug: "ai-crawler-robots-txt-strategy"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Decide which AI bots to allow or block in robots.txt, with reasoning per bot."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["generate-llms-txt", "sitemap-priority-for-large-sites", "ai-citation-worthiness-audit"]
---

## When to use this
You're setting up or auditing robots.txt and don't know which AI crawlers to name explicitly. Disallowing `*` blocks the answer-engine bots that could cite you; allowing everything hands your content to training scrapers you never meant to feed.

## The pattern
```text
I'm setting robots.txt rules for my site. Unless I tell you otherwise,
my goals are:
- get cited in live AI answers (ChatGPT search, Perplexity, Claude,
  AI Overviews)
- block bots that only scrape for LLM training data, not for answering
  live queries

For each of these AI-related crawlers, tell me whether to Allow or Disallow
it, with one line of reasoning tied to my goals above:
- GPTBot (OpenAI training crawler)
- OAI-SearchBot (OpenAI ChatGPT search/answers crawler)
- ChatGPT-User (OpenAI live browsing on behalf of a user)
- PerplexityBot (Perplexity search/answers crawler)
- ClaudeBot (Anthropic training crawler)
- Claude-SearchBot (Anthropic live web search crawler)
- Google-Extended (Gemini/AI Overviews training signal, separate from Googlebot)
- CCBot (Common Crawl, feeds many third-party LLM training sets)
- Bytespider (ByteDance crawler)
- Amazonbot (Amazon, feeds Alexa/shopping answers)
- Any other AI crawler you know is currently active that isn't listed above

Then output the complete robots.txt as a single code block: Allow rules
first, then Disallow rules, with a one-line comment above each rule stating
the decision's reason. Keep any existing non-AI rules (Googlebot, Bingbot,
sitemap directive) unchanged.

If you're running inside my site's codebase, read the current robots.txt
yourself (site root or public/). Otherwise, first ask me in one message to
paste my current robots.txt and state any goals that differ from the
defaults above, then wait. If I say there's no robots.txt yet, start fresh.
```

## Real example output
```
Decisions for docs.brightpath.io (goal: get cited in ChatGPT/Perplexity answers, keep training scrapers out):

- GPTBot: Disallow - trains OpenAI's base models, doesn't power live ChatGPT citations
- OAI-SearchBot: Allow - powers ChatGPT's live search citations, directly serves goal 1
- ChatGPT-User: Allow - user-triggered browsing, behaves like a real visitor
- PerplexityBot: Allow - powers Perplexity's answer citations
- ClaudeBot: Disallow - training crawler, same reasoning as GPTBot
- Claude-SearchBot: Allow - powers Claude's live web search citations
- Google-Extended: Allow - feeds AI Overviews, visibility there outweighs training opt-out
- CCBot: Disallow - feeds unknown downstream LLM training sets, no citation path back
- Bytespider: Disallow - aggressive scraping, no citation benefit for this audience
- Amazonbot: Disallow - not relevant, no shopping/Alexa surface applies here
```
```
User-agent: OAI-SearchBot
Allow: /

User-agent: ChatGPT-User
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: Claude-SearchBot
Allow: /

User-agent: Google-Extended
Allow: /

User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /

User-agent: CCBot
Disallow: /

User-agent: Bytespider
Disallow: /

User-agent: Amazonbot
Disallow: /

User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

Sitemap: https://docs.brightpath.io/sitemap.xml
```

## Why it works
"AI crawler" isn't one thing: training bots, live-answer bots, and user-triggered browsing bots have different user-agent strings and different consequences for you. Naming each one against a stated goal replaces a single blanket rule with a decision you can actually defend when someone asks why a page isn't showing up in an AI answer.
