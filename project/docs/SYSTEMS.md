# Systems

What exists in this project right now.
Update this file when structure, data flow, security boundaries, build, or deployment change.
Plans belong in `docs/PRODUCT.md` and `docs/ARCHITECTURE.md`, feature rationale in `docs/decisions/`, and working rules in `AGENTS.md` (see "Where things are written").
When a section grows past about 150 lines, split it into `docs/systems/<area>.md`.

## Current state

TODO(template): One or two sentences on what the repository contains today, and what does not exist yet (backend, database, authentication, build, tests).

| Path | Purpose |
|---|---|
| TODO(template): Each main application path | What it holds |
| `AGENTS.md` | Agent working agreement; `CLAUDE.md` is a symlink to it |
| `docs/PRODUCT.md` | Living product plan: vision, users, roadmap, and compliance status |
| `docs/ARCHITECTURE.md` | Living architecture plan: the parts, where each runs, and the reasons for each choice |
| `docs/agents/` | Task guides for agents, and `background.md` with the reasons behind the working rules |
| `docs/decisions/` | Feature decisions, one file each with a searchable header |
| `.claude/rules/` | Symlinks to path-scoped task guides, loaded automatically by Claude Code |
| `.agents/skills/` | Codex skills: `simplify`, `performance`, `security-review`, and pointer skills for path-scoped guides |
| `.github/pull_request_template.md` | Pull request description with the required review record |

## External dependencies

TODO(template): Services, APIs, fonts, and packages the project relies on at runtime, or "None".

## Deployment

TODO(template): Where it is hosted, what triggers a deploy, whether previews exist, and which files are excluded from what is served.

## Not yet decided

Open choices are listed under "Still open" in `docs/ARCHITECTURE.md`, and agents never decide them on their own.
