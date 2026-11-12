---
name: linear-project-sync
description: >-
  Use this skill when the user wants to synchronize, reflect, or manage their
  Antigravity workspace projects in Linear. Covers creating and updating Linear
  projects, issues, cycles, milestones, and status updates from the codebase
  context. Also use when the user asks to plan sprints, break down tasks, track
  progress, triage issues, or generate status reports that map to Linear.
---

# Linear Project Sync

Bridge your Antigravity workspace with Linear. Manage projects, issues, and
status updates through natural language — Linear is the source of truth, the
agent is the interface.

---

## Security & Safety Rules

1. **Verify before writing** — always call `whoami` before any write operation
   to confirm the correct workspace and identity.
2. **Never bulk-delete** — the agent must not delete issues, comments, projects,
   or labels. Deletion requires the user to act in the Linear UI directly.
3. **Confirm before bulk writes** — if creating or updating more than 3 issues
   in one go, present the plan to the user and wait for confirmation first.
4. **No sensitive data in descriptions** — never put API keys, tokens, secrets,
   passwords, or credentials into issue descriptions or comments.
5. **Respect existing data** — use `addLabels`/`removeLabels` instead of
   `labels` (which replaces the full set). Use `patch` instead of full
   `description` replacement when updating issues or documents.
6. **Check before creating** — always search for existing items before creating
   new ones to prevent duplicates.
7. **Scope to the workspace** — only read codebase files within the current
   workspace. Never reference or expose file paths outside the project root.
8. **Human-readable content only** — issue descriptions are for people. Put
   agent-specific detail in collapsible `+++ Agent notes` / `+++` sections.

---

## Pre-Flight (run before any workflow)

```
Tool: whoami                                    → confirm identity & workspace
Tool: list_teams                                → cache team names/keys
Tool: list_issue_statuses  { teamId: "<team>" } → learn valid statuses
Tool: list_issue_labels    { teamId: "<team>" } → learn valid labels
```

Statuses and labels differ per team — never assume names like "In Progress"
exist.

---

## Workflows

### 1. Project Setup

Trigger: "set up tracking", "scaffold Linear for this repo"

1. Read `README.md` / `package.json` / `pyproject.toml` for project context.
2. `list_projects { query: "<name>" }` — check for duplicates.
3. `save_project { name, description, teams: ["<team>"], state: "started" }`
4. Optionally create milestones with `save_milestone { project, name, targetDate }`.
5. Report back with the project identifier and URL.

### 2. Issue Creation

Trigger: "create an issue", "break this into tasks", "batch create issues"

1. **Duplicate check** — search with keyword variants before creating:
   ```
   list_issues { teamId: "<team>", query: "<term>" }
   list_issues { teamId: "<team>", query: "<synonym>" }
   ```
   Flag matches to the user. Don't silently create duplicates.

2. **Create** with structured descriptions:
   ```
   save_issue {
     team: "<team>",
     title: "Verb + noun (action-oriented)",
     description: "## Context\n...\n## Acceptance Criteria\n- [ ] ...",
     priority: 2,
     addLabels: ["Feature"]
   }
   ```

3. For sub-tasks, set `parentId` to the parent issue identifier.

4. For batch creation (>3 items): present the plan, get user confirmation,
   then create. Summarize results in a table.

### 3. Status Updates

Trigger: "write a status update", "Monday report", "how's the project going"

1. Gather state:
   ```
   get_project { nameOrId: "<project>" }
   list_issues { project: "<project>" }
   get_status_updates { type: "project", project: "<project>" }
   ```

2. Audit health — check for discrepancies (e.g., project "In Progress" but
   all issues in Backlog), unassigned issues, missing estimates.

3. Post update:
   ```
   save_status_update {
     type: "project",
     project: "<project>",
     health: "onTrack",        ← onTrack | atRisk | offTrack
     body: "## Progress\n...\n## Blockers\n...\n## Next Steps\n..."
   }
   ```

   Determine `health` from the audit — don't default to `onTrack`.

### 4. Sprint Planning

Trigger: "plan the sprint", "what's in the current cycle"

1. `list_cycles { teamId: "<team>", type: "current" }`
2. `list_issues { teamId: "<team>", cycle: "<cycle>" }` — show current load.
3. Suggest assignments based on priority and capacity.
4. Move issues: `save_issue { id: "ENG-42", cycle: "<cycle>" }`

### 5. Code → Linear Sync

Trigger: "mark done", "I merged the PR", "update the issue"

1. Find the issue: `list_issues { teamId: "<team>", query: "<branch-or-pr>" }`
2. Update status: `save_issue { id: "ENG-42", status: "Done" }`
3. Add context: `save_comment { issueId: "ENG-42", body: "Merged in PR #123" }`

### 6. Queries

Translate natural questions into tool calls:

| Question | Approach |
|:---------|:---------|
| "Show urgent bugs" | `list_issues { teamId, priority: 1, label: "Bug" }` |
| "What's unassigned?" | `list_issues { teamId }` → filter no assignee |
| "What's blocking us?" | `list_issues { teamId }` → check relations |
| "What moved this week?" | `list_issues { teamId, updatedAt: "-P7D" }` |

---

## Key Rules

- **Human-readable references** — Linear MCP accepts team names, emails, and
  identifiers like `ENG-42`. You rarely need raw IDs.
- **Paginate** — when `hasNextPage` is true, pass the returned `cursor`.
- **Literal content** — pass real newlines in markdown, not `\n` escape sequences.
- **Minimal writes** — don't update fields that don't need changing.

---

## Tool Reference

For full parameter details, see [linear-mcp-tools.md](./references/linear-mcp-tools.md).
