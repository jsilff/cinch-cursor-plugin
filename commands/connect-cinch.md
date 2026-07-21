---
name: connect-cinch
description: Open the Cursor MCP install deeplink so Cinch can authenticate with OAuth. Use when the user needs to connect or re-authenticate Cinch.
---

# Connect Cinch

Install and authenticate the hosted Cinch MCP server in Cursor.

## Install deeplink

Open this link (or paste it into a browser with Cursor installed):

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=cinch&config=eyJ0eXBlIjoiaHR0cCIsInVybCI6Imh0dHBzOi8vYXBwLmNpbmNoLndvcmsvbWNwIn0=
```

Cursor will prompt to add the **cinch** MCP server, then run OAuth in the browser.

## After connect

1. Confirm **cinch** shows tools under **Settings → Tools & MCP**.
2. Start a **new Agent chat**.
3. Try `list_projects` or `list_companies`.

## If already installed via the Cinch plugin

Open **Settings → Tools & MCP**, find **cinch**, click **Connect**, and complete OAuth. Plugin install alone does not always start the browser flow.

## Fallback

If the deeplink does not open Cursor, paste this into `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "cinch": {
      "url": "https://app.cinch.work/mcp"
    }
  }
}
```

Then restart Cursor and click **Connect** on **cinch**.
