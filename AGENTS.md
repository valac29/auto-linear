# Workspace Guidelines for auto-linear

## 1. Git Workflow
- **Branching**: Always create and work on dedicated feature branches (e.g. `feat/<feature-name>`). Do not commit directly to `main`.
- **Atomic Commits**: Keep commits focused and granular—one logical change per commit.
- **Commit Message Convention**:
  - Format: `<type>: <3 to 7 word description>`
  - Primary type: `feat: <3 - 7 word message>` (or `docs:`, `fix:`, `refactor:`)
  - Example: `feat: add linear project sync skill`
  - Example: `docs: update documentation and security rules`
- **Pull Requests**: Keep PR descriptions simple, concise, and focused on what was changed and why.

## 2. Security & Linear MCP Guardrails
- **Minimal Blast Radius**: Keep changes focused and avoid unnecessary dependencies or excessive complexity.
- **Identity Pre-Flight**: Run `whoami` before write operations to ensure operations target the right workspace and account.
- **No Destruction**: Never bulk-delete issues, projects, comments, or labels via the agent.
- **User Confirmation for Batch Ops**: Confirm with the user before creating or updating more than 3 issues at once.
- **Credential Safety**: Never leak or embed API keys, secrets, tokens, or passwords into Linear tickets or comments.
- **Duplicate Prevention**: Perform semantic duplicate checks prior to creating new tasks.

## 3. Project & Repository Architecture
- **1:1 Project-to-Repo Mapping**: Each Linear Project maps directly to 1 Git repository.
- **Monorepo for Complex Projects**: For complex systems with multiple services, packages, or apps, organize under a monorepo structure to keep Linear tracking, PR linking, and issue references consolidated and easy to track.
