---
name: novada
description: Use when you need live web data — searching the web, reading/extracting a page, crawling or mapping a site, scraping a platform (Amazon, LinkedIn, etc.), doing multi-source research, verifying a claim, or fetching a page blocked by anti-bot. Routes to the right Novada MCP tool.
---

# Novada — web data for agents

Novada exposes web-data tools via MCP. Pick the right tool by intent; don't guess URLs or scrape blindly.

## Decision tree

- **Find pages, don't know the URL** → `novada_search` (Google/Bing/DuckDuckGo/Yahoo/Yandex). Returns titles + URLs + snippets. Follow up with extract for full content.
- **Have a URL, want its content** → `novada_extract`. For the whole page use markdown; for **specific data points** (price, author, availability) pass `fields=[...]` instead of parsing markdown yourself.
- **Want many pages under a site/section** → `novada_crawl` (fetches + extracts, capped ~20 pages) or `novada_map` (just discover URLs, no content).
- **Structured data from a known platform** (Amazon, LinkedIn, Google Shopping, etc.) → `novada_scrape` with the platform + operation.
- **One question, many sources, synthesized** → `novada_research` (search → read → cite in one call).
- **Check whether a claim is supported** → `novada_verify`.
- **Page is blocked / JS-heavy / anti-bot** → use `novada_extract` with `render="render"`, or route via a proxy tool.

## Rules

- **Auth:** set `NOVADA_API_KEY` in the environment. Get a key at https://dashboard.novada.com/overview/scraper/api-playground/.
- **Format choice (extract):** specific data → `fields`; whole article → markdown. Don't extract full markdown then regex it yourself.
- **Errors carry guidance:** every Novada error includes an `agent_instruction` — read it and act on it (refresh key, switch render mode, retry) rather than giving up.
- **Don't over-fetch:** search → extract the one right URL beats crawling a whole site. Use `novada_map` to find the right page before extracting.
