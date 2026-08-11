---
name: cinch-setup
description: Connect Cinch to Cursor via the hosted MCP plugin or the local stdio MCP server. Use when the user needs help installing, authenticating, or troubleshooting Cinch MCP.
---

# Cinch Setup

## Recommended: one-click deeplink or Cursor plugin

**Fastest — MCP install deeplink** (opens Cursor and starts OAuth):

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=cinch&config=eyJ0eXBlIjoiaHR0cCIsInVybCI6Imh0dHBzOi8vYXBwLmNpbmNoLndvcmsvbWNwIn0=
```

Or run **`/connect-cinch`** after the plugin is installed.

**Plugin path:**

1. Install the **Cinch** plugin from Cursor Customize / Marketplace.
2. Open **Settings → Tools & MCP** and confirm **cinch** appears.
3. Click **Connect** (or use `/connect-cinch`) and complete OAuth in the browser.
4. Quit Cursor completely (Cmd+Q), reopen, and start a new Agent chat.
5. Verify tools such as `list_projects` and `list_tasks` are available.

The plugin registers the hosted MCP endpoint:

```json
{
  "mcpServers": {
    "cinch": {
      "type": "http",
      "url": "https://app.cinch.work/mcp"
    }
  }
}
```

OAuth runs via Connect / deeplink; no Personal Access Token is required for this path.

## Alternative: Local stdio MCP server

Use when OAuth is unavailable or the user prefers a PAT:

1. Clone [cinch-mcp-server](https://github.com/jsilff/cinch-mcp-server).
2. Run `npm install && npm run build`.
3. Create an **AI Assistant** token in Cinch → Settings → Personal Access Tokens.
4. Add to `~/.cursor/mcp.json` (user level — not project-only):

```json
{
  "mcpServers": {
    "cinch": {
      "command": "node",
      "args": ["/ABSOLUTE/PATH/TO/cinch-mcp-server/dist/index.js"],
      "env": {
        "CINCH_API_URL": "https://app.cinch.work",
        "CINCH_PAT": "cinch_xxxxxxxxxxxx"
      }
    }
  }
}
```

5. Quit and reopen Cursor; confirm 15 tools are listed under **cinch**.

Do **not** use the **Dashboard / Export** token preset for MCP — it is project-scoped and lacks write scopes needed by AI assistants.

## Claude Desktop

Claude supports remote MCP directly: **Customize → Connectors → Add custom connector** → paste `https://app.cinch.work/mcp`.

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Red dot / no tools | Check View → Output MCP logs; confirm URL ends with `/mcp`; restart Cursor |
| OAuth loop | Sign in to Cinch in the browser first, then retry Connect |
| Duplicate servers | Remove project-level `.cursor/mcp.json` Cinch entries; keep one user-level or plugin entry |
| Tools missing after install | Toggle cinch off/on in Tools & MCP; start a new Agent chat |

Hosted MCP docs: [MCP Remote Setup](https://github.com/jsilff/cinch-mcp-server) and Cinch Help → Connect Cinch to Your AI Tool.
