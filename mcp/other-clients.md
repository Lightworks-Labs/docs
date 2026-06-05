---
title: Connect any MCP client
---

The Lightworks MCP server speaks standard [Model Context Protocol](https://modelcontextprotocol.io) over HTTP with OAuth 2.0 + PKCE authentication, so it works with any MCP-compatible AI tool — not just the ones listed in the settings panel.

## Endpoint

```
https://app.lightworks.md/api/mcp
```

## Authentication

The server supports two authentication methods.

### OAuth 2.0 (recommended)

If your client supports OAuth 2.0 with PKCE, point it at the Lightworks authorization server:

| Parameter | Value |
|---|---|
| Authorization URL | `https://app.lightworks.md/oauth/authorize` |
| Token URL | `https://app.lightworks.md/api/oauth/token` |
| Scopes | `read:databases` · `read:databases write:databases` |
| PKCE | Required (`S256`) |

### Bearer token

Generate an API token from **Settings → API Tokens** and pass it as a header:

```
Authorization: Bearer YOUR_TOKEN
```

- **Read** tokens (`read:databases`) work on every plan
- **Write** tokens (`read:databases` + `write:databases`) require the paid plan

## Generic MCP config

Most clients accept a JSON config block. Replace `YOUR_TOKEN` with your API token or use OAuth if supported:

```json
{
  "mcpServers": {
    "lightworks": {
      "type": "http",
      "url": "https://app.lightworks.md/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

Some clients use a `url` key without `type`:

```json
{
  "mcpServers": {
    "lightworks": {
      "url": "https://app.lightworks.md/api/mcp"
    }
  }
}
```

Refer to your client's MCP documentation for the exact config format.

## Available tools

See the [MCP Server overview](/docs/mcp) for the full list of tools and a description of the read/write PR flow.

## Related

- [MCP Server](/docs/mcp) — full tool reference and typical agent workflow
- [Developers](/docs/developers) — REST API and GitHub Actions integration
- [API Tokens](/docs/settings) — generate and manage tokens
