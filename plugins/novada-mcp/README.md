# Novada MCP

Novada MCP connects Claude to Novada's hosted web-data platform: search, scraping,
extraction, crawling, browser automation, and account management, all through one
remote MCP server at `https://mcp.novada.com/mcp`. The plugin ships no local code —
it only registers that remote connection and authenticates it with your own Novada
account through OAuth sign-in. There is no local server process, no `npx`, and no
API key to paste into a config file.

## Tools (30)

- `novada_search` — Web search returning titles, URLs, and snippets, reranked by relevance.
- `novada_extract` — Extract content from any URL, with automatic anti-bot escalation (static fetch, then JS rendering, then a managed browser).
- `novada_crawl` — Extract content from up to 20 pages under a site section in one call.
- `novada_research` — Run several parallel searches across engines and return cited source passages for a question.
- `novada_map` — Discover a site's URLs via sitemap or crawl, without fetching page content.
- `novada_scrape` — Structured data (not raw HTML) for one of 16 supported platforms, selected by an `operation` parameter.
- `novada_scrape_amazon` — Structured Amazon product, review, seller, and bestseller data.
- `novada_scrape_google` — Structured Google web search, AI Mode, Maps, Shopping, Jobs, Hotels, and Videos results.
- `novada_scrape_bing` — Structured Bing web, video, news, and shopping results.
- `novada_scrape_duckduckgo` — Structured DuckDuckGo web search results.
- `novada_scrape_yandex` — Structured Yandex web search results.
- `novada_scrape_youtube` — YouTube video, channel, transcript, and comment data.
- `novada_scrape_instagram` — Instagram profile, post, reel, and comment data.
- `novada_scrape_facebook` — Public Facebook profile, post, comment, and event data.
- `novada_scrape_tiktok` — TikTok profile and post data, including hashtag and search listings.
- `novada_scrape_x` — X (Twitter) post and profile lookups.
- `novada_scrape_walmart` — Structured Walmart product data by keyword, category, SKU, or URL.
- `novada_scrape_shein` — Structured SHEIN product data by product ID or URL.
- `novada_scrape_linkedin` — LinkedIn job listings and company profile data.
- `novada_scrape_github` — GitHub repository metadata, including search-result listings.
- `novada_scrape_perplexity` — The generated answer Perplexity returns for a query.
- `novada_proxy` — Issue a residential or mobile proxy endpoint for the caller's own HTTP requests.
- `novada_browser` — Drive a web page interactively: click, fill forms, scroll, screenshot, or run JavaScript.
- `novada_discover` — List every tool this server exposes, with category and status.
- `novada_ai_monitor` — Search public pages on AI-company domains for brand mentions and sentiment.
- `novada_monitor` — Track a page over time and report what changed since the last check.
- `novada_setup` — Onboarding check that validates an API key and reports account status.
- `novada_account` — Account and billing dashboard: wallet balance, plan balances, and recent usage.
- `novada_proxy_account_create` — Create a proxy sub-account under the caller's plan.
- `novada_proxy_account_list` — List the proxy sub-accounts under the caller's plan.

## Connect

**Claude Code**

1. Install the plugin from this marketplace.
2. Run `/mcp` and select the `novada` server.
3. Follow the sign-in prompt to authenticate with your Novada account via OAuth.

**claude.ai and Claude Desktop**

1. Open the plugin's Connectors tab.
2. Select Connect.
3. Sign in with your Novada account when prompted.

No environment variable and no API key need to be set by hand on any surface —
authentication happens through the OAuth sign-in flow described above.

## Network and data disclosure

This plugin runs no local code and starts no local process. Every tool call goes
over HTTPS to `https://mcp.novada.com/mcp`, which is a stateless HTTP wrapper Novada
operates in front of its own backend services.

**Authentication.** The server implements the standard MCP OAuth flow: discovery at
`https://mcp.novada.com/.well-known/oauth-authorization-server` and
`https://mcp.novada.com/.well-known/oauth-protected-resource`, dynamic client
registration at `https://mcp.novada.com/register`, authorization at
`https://mcp.novada.com/authorize`, and token exchange at
`https://mcp.novada.com/token`. You paste your own Novada API key once, during the
consent step; Claude never asks you to put that key in a config file or environment
variable.

**What is stored.** Your Novada API key is encrypted at rest (AES-256-GCM, a fresh
nonce per record) inside the OAuth records the server issues, never stored in
plaintext. Authorization codes expire after 2 minutes; access tokens expire after
1 hour; refresh tokens expire after 30 days and rotate on every use; client
registrations expire after 180 days.

**Backend hosts this server calls.** Depending on which tool you invoke, the hosted
MCP server forwards your request to one of Novada's own backend hosts:
`https://api-m.novada.com` (account, wallet, and key verification),
`https://scraper.novada.com` (the platform-scraping tools), and
`https://webunlocker.novada.com` (extraction, crawling, mapping, and browser
automation). A legacy result-download path also uses `https://api.novada.com`.

**Target URLs you ask it to fetch.** When you ask a tool to read, scrape, or crawl
a URL (for example, a product page or a search-engine result), that page is
fetched through Novada's own scraping and unblocking infrastructure, not directly
from your machine, and the response is returned to Claude through the same
`https://mcp.novada.com/mcp` connection.

**Telemetry.** The hosted server records operational metadata for each tool call:
the tool name, the argument key names (never argument values, with one exception —
four allowlisted fields, `operation`, `platform`, `render`, and `format`, whose
closed enum values are recorded), the hostname only of any target URL (never its
path, query string, port, or credentials), outcome, latency, and an encrypted
identity token derived from your API key. It never records argument values, search
query text, full target URLs, or raw error messages. If the telemetry backend is
not configured, no telemetry calls happen at all.

**Policies and support.** Privacy policy: `https://novada.com/privacy`. Support:
`support@novada.com`.
