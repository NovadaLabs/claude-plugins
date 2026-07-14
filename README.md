# Novada — Claude Code Plugin Marketplace

Official [Novada](https://novada.com) plugin marketplace for Claude Code. Ships one plugin: **novada-mcp**, which connects Claude Code to the Novada hosted MCP server at [mcp.novada.com/mcp](https://mcp.novada.com/mcp).

23 tools across web search, scraping, extraction, crawling, site mapping, browser automation, multi-source research, claim verification, account management, and proxy provisioning — all behind a single `NOVADA_API_KEY`.

## 2-step install

```
/plugin marketplace add NovadaLabs/novada-claude-plugins
/plugin install novada-mcp@novada
```

Then set your API key:

```bash
export NOVADA_API_KEY=<your-key>
```

Get a key at <https://dashboard.novada.com/api-key/> — 1,000 free calls/month on the free plan; paid accounts are uncapped.

## How it works

The plugin wires Claude Code to the **hosted** Novada MCP endpoint (`https://mcp.novada.com/mcp`). No `npx`, no local server process, no extra dependencies. Claude Code starts the connection automatically when the plugin is active.

Your key is passed as a `Bearer` token on every request. The hosted server validates and bills against your own Novada balance.

## Manual MCP config (no plugin required)

If you prefer to configure MCP directly instead of using the plugin system:

**Hosted endpoint (recommended):**

```json
{
  "mcpServers": {
    "novada": {
      "type": "http",
      "url": "https://mcp.novada.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_NOVADA_API_KEY"
      }
    }
  }
}
```

Save this to `.mcp.json` at your project root (team-shared, checked into version control) or add via CLI:

```bash
claude mcp add --transport http novada https://mcp.novada.com/mcp \
  --header "Authorization: Bearer YOUR_NOVADA_API_KEY"
```

**Local server (alternative, requires Node):**

```json
{
  "mcpServers": {
    "novada": {
      "command": "npx",
      "args": ["-y", "novada-mcp@latest"],
      "env": {
        "NOVADA_API_KEY": "YOUR_NOVADA_API_KEY"
      }
    }
  }
}
```

Or via CLI:

```bash
claude mcp add --transport stdio novada \
  --env NOVADA_API_KEY=YOUR_KEY -- npx -y novada-mcp@latest
```

## Links

- MCP endpoint: <https://mcp.novada.com/mcp>
- Product page: <https://novada.com/products/novada-mcp/>
- npm package: [novada-mcp](https://www.npmjs.com/package/novada-mcp)
- API key: <https://dashboard.novada.com/api-key/>

## License

MIT © NovadaLabs
