---
name: cinch-mcp
description: Use Cinch MCP tools to manage projects, tasks, and comments. Load this skill before calling any Cinch MCP tool when the user asks about Cinch work, task tracking, project planning, or status updates.
---

# Cinch MCP

Use the Cinch MCP server to read and write project management data in [Cinch](https://app.cinch.work).

## When to use

- Listing or searching projects and tasks
- Creating or updating tasks, projects, and comments
- Checking organization membership or project details
- Summarizing work status across a team or project

## Prerequisites

1. The Cinch plugin MCP server must be connected (Settings → Tools & MCP → **cinch**).
2. Complete OAuth when prompted on first connect.
3. Start a **new Agent chat** after connecting if tools are not visible.

## Workflow

1. **Discover scope** — call `list_companies` when the user has not named an organization.
2. **Find context** — use `list_projects` and `list_tasks` before creating or updating records.
3. **Read before write** — call `get_task` or `get_project` before destructive or broad updates.
4. **Confirm IDs** — never guess project or task IDs; list first, then act on returned IDs.
5. **Paginate** — pass `cursor` from list responses when results may be truncated.
6. **Preserve schedule metadata** — when changing a timed task, carry forward
   `dueTimeZone` and `startTimeZone` from `get_task` unless the user explicitly
   asks to remove the time.

## Tools

| Tool | Purpose |
|------|---------|
| `list_companies` | Organizations the user belongs to |
| `get_company` | Members and groups for one organization |
| `list_projects` | Projects (optional `companyId`, `groupId`, `includeArchived`) |
| `get_project` | Project details, members, custom fields |
| `create_project` | New project (`name`, `key`, optional `description`, `color`) |
| `list_tasks` | Filter by `projectId`, `status`, `assigneeId`, `parentId` |
| `get_task` | Full task with comments, tags, subtasks |
| `create_task` | New task in a project |
| `update_task` | Change status, assignee, dates, title, etc. (not project) |
| `copy_task` | Copy a task and its subtasks to another project |
| `move_task` | Move a task and its subtasks to another project |
| `bulk_copy_tasks` | Copy multiple tasks from one project to another |
| `bulk_move_tasks` | Move multiple tasks from one project to another |
| `list_comments` | Thread on a task |
| `create_comment` | Add comment (supports @mentions in content) |

## Task status values

Use these enum values in tool arguments:

- `BACKLOG`, `TODO`, `IN_PROGRESS`, `IN_REVIEW`, `DONE`, `CANCELLED`

Show Title Case labels to the user (e.g. **In Progress**), not raw enums.

## Common patterns

**List my open tasks in a project**

1. `list_projects` → find `projectId`
2. `list_tasks` with `projectId` and `status: "IN_PROGRESS"` (repeat for `TODO` if needed)

**Create a task with a comment**

1. `create_task` with `projectId`, `title`, optional `description`, `status`
2. `create_comment` with returned task `id`

**Move task to done**

1. `get_task` to confirm the correct record
2. `update_task` with `status: "DONE"`

**Copy a task to another project**

1. `list_projects` → find source and `targetProjectId`
2. `get_task` or `list_tasks` to confirm the task id
3. `copy_task` with `{ taskId, targetProjectId }`

**Move a task to another project**

1. `list_projects` → find source and `targetProjectId`
2. `get_task` to confirm the task and review its subtasks (moves include the full subtree)
3. `move_task` with `{ taskId, targetProjectId }`

**Bulk copy or move tasks**

1. `list_projects` → find source `projectId` and `targetProjectId`
2. `list_tasks` / `get_task` as needed to gather ids
3. For moves, call `get_task` on each root you intend to move so subtasks are visible before the destructive transfer
4. `bulk_copy_tasks` or `bulk_move_tasks` with `{ projectId, taskIds, targetProjectId }`

**Create or update timed tasks**

- Cinch title syntax accepts `today @ 3`, `Jan 3 at 3:30pm`,
  `on the 5th in the morning`, and equivalent date/time phrases.
- For direct tool fields, pair a timed ISO value with its source IANA timezone:
  `dueDate: "2027-01-03T20:30:00.000Z"` and
  `dueTimeZone: "America/New_York"`.
- Date-only values omit the timezone field.
- A recurring timed task uses the stored source timezone to keep the same local
  wall-clock time through daylight-saving transitions.
- To remove a due time but retain its date, first read the task, then call
  `update_task` with `dueTimeZone: null` and omit `dueDate`.
- When reading a task, treat a non-null `dueTimeZone`/`startTimeZone` as the
  signal that its corresponding ISO date is timed rather than date-only.

## Troubleshooting

- **No tools available**: Quit Cursor completely (Cmd+Q), reopen, toggle **cinch** in Tools & MCP, start a new Agent chat.
- **401 / auth errors**: Click Connect on the cinch server and complete OAuth in the browser.
- **Wrong organization**: Pass `companyId` from `list_companies` on list/create calls.
- **Stdio fallback**: For PAT-based local setup, see the [cinch-mcp](https://github.com/jsilff/cinch-mcp-server) repo and the `cinch-setup` skill.
