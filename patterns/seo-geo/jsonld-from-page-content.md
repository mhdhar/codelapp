---
title: "Generate JSON-LD Schema From Existing Page Content"
slug: "jsonld-from-page-content"
format: "prompt"
category: "seo-geo"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Paste a page's content, get back valid JSON-LD structured data ready to embed."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You have a page live with no structured data, and you want search engines and LLMs to parse its content correctly (rich results, better citations). Use this per page type: article, product, FAQ, how-to.

## The pattern
```text
Here is the content of a page at [PAGE_URL]:

[PAGE_CONTENT]

Generate valid JSON-LD structured data for this page:
- Pick the single most accurate schema.org type for this content (Article,
  Product, FAQPage, HowTo, etc.), don't guess a generic type if a specific
  one fits better
- Extract only facts that are actually present in the content above, do not
  invent prices, ratings, dates, or author names that aren't there
- Include all required and recommended properties for that schema type
- Output as a single <script type="application/ld+json"> block ready to
  paste into the page <head>
- After the code block, list any required properties you could NOT fill in
  because the source content didn't include them, so I know what to add
```

## Real example output
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Generate an llms.txt File for Your Site",
  "step": [
    {
      "@type": "HowToStep",
      "name": "List your key pages",
      "text": "Gather URLs and one-line descriptions of your site's most important pages."
    },
    {
      "@type": "HowToStep",
      "name": "Generate the file",
      "text": "Use an AI prompt to format the list into a structured llms.txt file."
    },
    {
      "@type": "HowToStep",
      "name": "Publish at site root",
      "text": "Save the file as /llms.txt so crawlers can find it at a predictable path."
    }
  ]
}
</script>

Missing (not in source content): totalTime, author name.
```

## Why it works
Schema.org's `@type` vocabulary is precise, and picking the wrong one (Article for a Product page) is worse than no schema at all. Grounding extraction strictly in the pasted content, and flagging gaps instead of filling them with invented data, keeps the output valid and avoids penalties for misleading structured data.
