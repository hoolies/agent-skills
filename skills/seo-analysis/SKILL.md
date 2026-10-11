---
name: seo-analysis
description: >-
  Full search audit of a site: local technical SEO in the repo, plus a live
  pass for rankings, indexing, metadata, schema, and Core Web Vitals. Use when
  the user asks for an SEO audit, search rankings, organic traffic, Search
  Console, indexing, meta tags, sitemaps, schema, or Core Web Vitals. Produce
  one report with the few changes that will move organic traffic.
---

# Full search audit

One audit, two layers. The technical layer always runs. The search layer runs when a live URL exists, and it uses Search Console when the user can grant access. Do not stop after the local checklist.

## 1. Pin the site

Find the canonical URL from the repo (`homepage`, `site`, `baseURL`, `SITE_URL`, `CNAME`) or ask for it. If the repo URL and the URL the user named differ, say so and wait.

Store it as the target URL for the rest of the audit.

## 2. Technical layer

Run this against the repo and, when the URL responds, against the live HTML. Fix findings in the repo when the user asked for fixes. Otherwise report them.

### Meta tags

Every indexable page needs a unique title (50–60 characters), a unique meta description (120–155 characters), a self-referencing canonical, and a viewport tag. Flag `noindex` on pages that should rank.

### Open Graph

`og:title`, `og:description`, `og:image` (1200×630), `og:url`, `og:type`, and `twitter:card`. Missing tags mean a blank share preview.

### Structured data

Extract JSON-LD `@type` values. Compare them to the site type:

| Site | Expected types |
|------|----------------|
| Shop | Product, Offer, BreadcrumbList |
| Local business | LocalBusiness, OpeningHoursSpecification |
| Software | Organization, SoftwareApplication |
| Articles | Article or BlogPosting, BreadcrumbList |

Flag missing required fields, relative URLs, and schema that describes content the page does not show.

### Sitemap and robots

`/sitemap.xml` lists indexable URLs, includes `lastmod`, and stays under 50,000 URLs. `/robots.txt` points at the sitemap, does not block CSS or JS, and does not block pages that should rank.

### Crawl and indexability

Important pages are reachable by internal links. No orphan pages, no redirect chains, no broken internal links. 404s return HTTP 404. Heading order starts at one `h1`. Images have `alt`. Links are descriptive. `<html lang>` is set.

### Core Web Vitals

| Metric | Good | Poor |
|--------|------|------|
| LCP | under 2.5s | over 4.0s |
| INP | under 200ms | over 500ms |
| CLS | under 0.1 | over 0.25 |

Check render-blocking scripts, image size and format, font preload, and layout shift from late content.

Live checks when the URL responds:

```bash
curl -sS "$URL/robots.txt"
curl -sS "$URL/sitemap.xml" | head
curl -sS "$URL" | grep -E '<title>|<meta name="description"|<link rel="canonical"|application/ld\+json'
curl -o /dev/null -sS -w "%{http_code}\n" "$URL"
```

## 3. Search layer

### PageSpeed

Call the PageSpeed Insights API for the homepage and up to four important URLs, mobile and desktop. No key is required for a few URLs. If `PAGESPEED_API_KEY` is set, send it as `key`.

```bash
curl -sS "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=${URL}&strategy=mobile&category=performance&category=seo"
```

Report the performance score, LCP, INP, and CLS from field data when Chrome UX Report has it, otherwise from the lab run. Name the top opportunity for each weak URL.

### Search Console

Use Search Console when `gcloud` is already authenticated for the webmasters scope, or when the user pastes an export. Do not invent rankings.

Pull the last 90 days:

- Queries and pages by clicks, impressions, CTR, and position
- Queries sitting in positions 4–20 (striking distance)
- Queries and pages that lost clicks versus the prior 28 days
- Queries where more than one URL ranks (cannibalization)
- Branded versus non-branded queries, once the user names the brand

If Search Console is not available, say that rankings are missing and continue. Do not block the technical report on it.

### Indexing

For the homepage and pages that lost traffic, note whether they are indexable: `noindex`, canonical pointing elsewhere, blocked in robots, or absent from the sitemap. When URL Inspection is available, use it. Otherwise infer from the live HTML and robots file.

## 4. One report

Lead with the three to five changes that will move organic traffic on this site. Then the evidence.

```markdown
# Search audit — [host]

## Do these first
1. [Change] — [page] — [why it moves traffic]

## Rankings
[Striking-distance queries, losses, cannibalization. Or: Search Console was not available.]

## Technical
[Meta, schema, sitemap, robots, crawl, vitals. Table of page, title length, description, canonical, schema.]

## Left alone
[Real issues that are not worth doing before the list above.]
```

Every recommendation names the URL and the check that failed. Do not tell the user to run a second SEO skill.
