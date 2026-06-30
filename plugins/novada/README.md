# Novada plugin for Claude Code

Web data for your agent — **search, scrape, crawl, extract, research & proxy** — via the [Novada](https://www.novada.com) MCP server, plus a skill that routes to the right tool.

## Install

```
/plugin marketplace add NovadaLabs/Novada-plugins
/plugin install novada@novada
```

Then set your key (the plugin's MCP server reads it from the environment):

```
export NOVADA_API_KEY=nvd_...
```

Get a key at <https://dashboard.novada.com/overview/scraper/api-playground/>.

## What you get

- **MCP tools** (default: local `npx -y novada-mcp`): `novada_search`, `novada_extract`, `novada_crawl`, `novada_map`, `novada_research`, `novada_scrape`, `novada_verify`, and more.
- **A skill** that tells Claude *when* to use *which* tool (search vs extract vs crawl vs scrape vs research).

## Hosted instead of local

To use the hosted server (no `npx`/Node), replace `.mcp.json` with:

```json
{
  "mcpServers": {
    "novada": {
      "type": "http",
      "url": "https://mcp.novada.com/mcp",
      "headers": { "Authorization": "Bearer ${NOVADA_API_KEY}" }
    }
  }
}
```

## License

MIT © Novada
