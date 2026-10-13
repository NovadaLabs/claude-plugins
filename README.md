# Novada — Claude Code Plugin Marketplace

Official [Novada](https://novada.com) plugin marketplace for Claude Code. Ships one plugin: **novada-mcp**, which connects Claude Code to the Novada hosted MCP server at [mcp.novada.com/mcp](https://mcp.novada.com/mcp).

29 tools across web search, scraping, extraction, crawling, site mapping, multi-source research, account management, and proxy provisioning.

## 2-step install

```
/plugin marketplace add NovadaLabs/claude-plugins
/plugin install novada-mcp@novada
```

Then run `/mcp` and sign in with your Novada account when prompted. No API key needs to be pasted into any config file or environment variable — the plugin authenticates over OAuth. Don't have a Novada account yet? Create one at <https://novada.com> — 1,000 free calls/month on the free plan; paid accounts are uncapped.

## How it works

The plugin wires Claude Code to the **hosted** Novada MCP endpoint (`https://mcp.novada.com/mcp`). No `npx`, no local server process, no extra dependencies. Claude Code starts the connection automatically when the plugin is active, then prompts an OAuth sign-in the first time you use it.

See [plugins/novada-mcp/README.md](https://github.com/NovadaLabs/claude-plugins/blob/main/plugins/novada-mcp/README.md) for the full tool list and the network and data disclosure.

## Manual MCP config (no plugin required)

If you prefer to configure MCP directly instead of using the plugin system, the hosted endpoint also accepts a Novada API key as a `Bearer` token — this path is independent of the plugin's OAuth flow and suits a project `.mcp.json` you check into version control:

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

MIT © Novada
