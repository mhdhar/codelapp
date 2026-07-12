---
title: "Segment and Freshen a Sitemap for a Large Site"
slug: "sitemap-priority-for-large-sites"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Turn a flat URL list into a segmented sitemap with accurate canonical URLs and lastmod values."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["ai-crawler-robots-txt-strategy", "internal-linking-gap-analysis", "canonical-duplicate-content-resolver"]
---

## When to use this
Your site has thousands of URLs and sitemap.xml is one flat file with stale or indiscriminate metadata. Use this before regenerating the sitemap; segmentation and canonical-URL decisions drive a maintainable file structure.

## The pattern
```text
I'm restructuring my site's sitemap. The input is a sample export of my
pages (URL, page type, last content update date, 90-day organic
sessions). Take the domain from the URLs in the export.

Do the following:
1. Group the URLs into logical sitemap segments by page type (e.g.
   /sitemap-products.xml, /sitemap-blog.xml, /sitemap-docs.xml). Tell me
   which segments should exist based on the page types actually present.
2. Flag URLs for sitemap review: obvious duplicates, paginated URLs
   (?page=2, ?sort=), known noindex pages, or non-canonical URLs. Do not
   exclude a page merely because it has zero sessions or is old; the supplied
   data cannot establish whether it should be indexed.
3. Use <lastmod> only when the supplied date is the page's real, meaningful
   content update date. Omit it when unknown.
4. Omit <priority> and <changefreq>; do not invent crawl signals from traffic
   density or page type.
5. Output a sitemap index file (sitemap.xml) referencing each segment file,
   plus one fully worked example segment file with 3-5 real URLs from the
   data, showing correct absolute <loc> values and <lastmod> only where known.

Use actual <lastmod> dates from the data provided. Do not use today's date
as a placeholder for pages that haven't changed.

First ask me in one message for the page inventory export and the
approximate total URL count, then wait. If I skip the count, treat the
sample as representative and say so in your output.
```

## Real example output
```
brightpath.io: 4,200 URLs sampled. Segments identified:
- /sitemap-products.xml (1,800 URLs): canonical product pages
- /sitemap-docs.xml (1,300 URLs): canonical documentation pages
- /sitemap-blog.xml (900 URLs): canonical blog posts
- /sitemap-legal.xml (12 URLs): canonical legal pages

Review list (188 URLs): 142 paginated product listing pages (`?page=N`),
31 internal search-result pages indexed by accident, and 15 blog posts that
need a canonical/indexability check. Zero sessions or an old date alone does
not decide whether a page belongs in the sitemap.
```
```xml
<!-- sitemap.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap><loc>https://brightpath.io/sitemap-products.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-docs.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-blog.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-legal.xml</loc></sitemap>
</sitemapindex>

<!-- sitemap-products.xml (excerpt) -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://brightpath.io/products/invoice-automation</loc>
    <lastmod>2026-06-14</lastmod>
  </url>
</urlset>
```

## Why it works
A sitemap should list the canonical, indexable URLs you want in search. Segmenting by page type can make large files manageable, while truthful <lastmod> values help crawlers understand real content changes. Traffic and age alone are not evidence that a URL should be removed.
