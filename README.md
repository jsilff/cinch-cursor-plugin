# Cinch Cursor Plugin

Official [Cursor](https://cursor.com) plugin for [Cinch](https://app.cinch.work) project management. Connects to Cinch's hosted MCP server with OAuth, plus skills and rules so agents manage projects and tasks reliably.

## Features

### MCP Server Integration

- One-click connection to `https://app.cinch.work/mcp` (OAuth)
- 11 tools: projects, tasks, comments, and organizations
- Organization resources for multi-company accounts

### Skills

- **cinch-mcp** — Workflow guidance for all Cinch MCP tools
- **cinch-setup** — Install, OAuth, and troubleshooting

### Commands

- **`/connect-cinch`** — Open the MCP install deeplink and complete OAuth

### Rules

- **cinch-task-labels** — Title Case labels for statuses and projects in user-facing text

## Installation

### One-click MCP install (OAuth)

Open this deeplink to add the hosted Cinch MCP server and start OAuth:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=cinch&config=eyJ0eXBlIjoiaHR0cCIsInVybCI6Imh0dHBzOi8vYXBwLmNpbmNoLndvcmsvbWNwIn0=
```

Or run the **`/connect-cinch`** command after installing the plugin.

### From Cursor Marketplace

Install the **Cinch** plugin from Cursor **Customize**, then:

1. Open **Settings → Tools & MCP** (or use `/connect-cinch`)
2. Connect **cinch** and complete OAuth in the browser
3. Quit Cursor completely (Cmd+Q), reopen, and start a new Agent chat

### Local Testing

Cursor supports two local test flows:

**Option A — Copy or symlink into the local plugins folder (fastest iteration)**

```bash
ln -s "/Users/jonathan/Dropbox/Fearless Future/Project Management Web App/cinch-cursor-plugin" \
  ~/.cursor/plugins/local/cinch
```

Then restart Cursor or run **Developer: Reload Window**. The plugin appears under **Customize → Installed**.

**Option B — Import the repo as a team marketplace**

Requires `.cursor-plugin/marketplace.json` at the repo root (included in this repo). Use **Dashboard → Plugins → Add Marketplace → Import from Repo**, or point Cursor at your GitHub repo URL.

After changing plugin files, restart Cursor or reload the window.

Validate before publishing:

```bash
npm run validate
```

## MCP Tools

| Tool | Description |
|------|-------------|
| `create_project` | Create a new project |
| `list_projects` | List projects |
| `get_project` | Project details |
| `create_task` | Create a task |
| `list_tasks` | List/filter tasks |
| `get_task` | Task details |
| `update_task` | Update a task |
| `create_comment` | Add a comment |
| `list_comments` | List comments |
| `list_companies` | List organizations |
| `get_company` | Organization details |

## Stdio Fallback

For PAT-based local setup without OAuth, use the separate [cinch-mcp-server](https://github.com/jsilff/cinch-mcp-server) repo. See the **cinch-setup** skill for configuration.

## Project Structure

```
cinch-cursor-plugin/
├── .cursor-plugin/
│   ├── marketplace.json  # Required for repo / team marketplace import
│   └── plugin.json       # Plugin manifest
├── assets/
│   └── logo.svg
├── mcp.json              # Hosted MCP server config
├── commands/
│   └── connect-cinch.md
├── rules/
│   └── cinch-task-labels.mdc
├── skills/
│   ├── cinch-mcp/
│   └── cinch-setup/
├── scripts/
│   └── validate-plugin.mjs
├── LICENSE
└── README.md
```

## Publishing

1. Run `npm run validate`
2. Push to GitHub
3. Submit to the Cursor team (Slack or marketplace submission process)

## Related Repos

- [cinch-mcp-server](https://github.com/jsilff/cinch-mcp-server) — Local stdio MCP bridge (PAT)
- [Cinch](https://app.cinch.work) — Project management app

## License

MIT — see [LICENSE](./LICENSE).
