# Linear MCP — Key Tool Parameters

Consult this only when you need exact parameter names or allowed values.

## Write Tools

### `save_issue` (create: omit `id` / update: pass `id`)
| Param | Notes |
|:------|:------|
| `team` | Required on create. Name, key, or ID |
| `title` | Required on create (unless `template` set) |
| `id` | Issue ID or identifier (e.g., ENG-42) for updates |
| `description` | Markdown. Literal newlines, no escape sequences |
| `status` | Status name — verify with `list_issue_statuses` first |
| `assignee` | User name, email, or "me". Null to remove |
| `priority` | 0=none, 1=urgent, 2=high, 3=normal, 4=low |
| `addLabels` | Append labels (preferred over `labels`) |
| `removeLabels` | Remove specific labels |
| `parentId` | Parent issue for sub-tasks |
| `cycle` | Cycle name/number/ID |
| `dueDate` | ISO date |
| `patch` | Incremental edits (update only, instead of full `description`) |

### `save_project` (create: omit `id` / update: pass `id`)
| Param | Notes |
|:------|:------|
| `name` | Required on create |
| `description` | Markdown |
| `teams` | Array of team names/IDs |
| `state` | planned / started / paused / completed / canceled |
| `lead` | User name, email, or "me" |
| `targetDate` | ISO date |

### `save_status_update`
| Param | Notes |
|:------|:------|
| `type` | Required: "project" or "initiative" |
| `project` | Required on create (project name/ID/identifier) |
| `body` | Required on create. Markdown |
| `health` | onTrack / atRisk / offTrack |

### `save_comment` (create: omit `id` / update: pass `id`)
| Param | Notes |
|:------|:------|
| `issueId` | Required on create |
| `body` | Required on create. Markdown |

### `save_milestone`
| Param | Notes |
|:------|:------|
| `project` | Required. Project name/ID/identifier |
| `name` | Required on create |
| `targetDate` | ISO date |

## Read Tools

- `whoami` — No params. Returns user, workspace, connection info.
- `list_teams` — No params.
- `list_projects` — Key filters: `query`, `team`, `state`, `member`.
- `list_issues` — Key filters: `team`, `query`, `status`, `assignee`, `label`, `project`, `cycle`, `priority`. Max 50 per page.
- `list_issue_statuses` — `team` required.
- `list_issue_labels` — `team` required.
- `list_cycles` — `teamId` required. Optional `type`: current/previous/next.
- `list_milestones` — `project` required.
- `get_status_updates` — `type` required. Optional: `project`, `user`.
- `get_notifications` — No params.

## Priority Values

| Value | Meaning |
|:------|:--------|
| 1 | Urgent |
| 2 | High |
| 3 | Normal |
| 4 | Low |
| 0 | None |
