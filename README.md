# auto-linear

AI-powered project management bridge between Antigravity and Linear.

## What It Does

An Antigravity skill that lets you manage Linear projects through natural
language — creating issues, planning sprints, writing status updates, and
querying your backlog without opening Linear.

## Quick Start

```
"Set up Linear tracking for this project"
"Create an issue for the auth bug"
"Write a Monday status report"
"What's in the current sprint?"
"Show me all urgent bugs"
```

## Prerequisites

- Linear MCP plugin installed and signed in in Antigravity

## Structure

```
.agents/skills/linear-project-sync/
├── SKILL.md                         # Skill instructions
└── references/linear-mcp-tools.md   # Tool parameter reference
```

## Project Tracking & Milestones

- **Linear Project**: [Auto Linear (P-GHO-1)](https://linear.app/ghost-automation/project/auto-linear-2be49b0e51ad)
- **Active Milestone**: `v1.0 MVP - Antigravity Linear Bridge` (Target: 2026-11-20)
- **Active Issues**:
  - [GHO-5](https://linear.app/ghost-automation/issue/GHO-5/implement-linear-project-sync-skill-for-antigravity): Implement linear-project-sync skill for Antigravity
  - [GHO-6](https://linear.app/ghost-automation/issue/GHO-6/establish-workspace-security-guardrails-and-git-workflow-rules): Establish workspace security guardrails and Git workflow rules

## Security

- Confirms identity (`whoami`) before any write operation
- Never bulk-deletes — deletion happens in Linear UI only
- Confirms with user before creating more than 3 issues at once
- Never puts secrets or credentials in issue descriptions
- Uses incremental label/description updates to avoid data loss
- Checks for duplicates before creating new items
