# Systems

What exists in this project right now.
Update this file when structure, data flow, security boundaries, build, or deployment change.
History and rationale belong in `docs/decisions/`.
When a section grows past about 150 lines, split it into `docs/systems/<area>.md`.

## Current state

TODO(template): One or two sentences on what the repository contains today, and what does not exist yet (backend, database, authentication, build, tests).

| Path | Purpose |
|---|---|
| TODO(template): Each main application path | What it holds |
| `AGENTS.md` | Agent working agreement; `CLAUDE.md` is a symlink to it |
| `docs/agents/` | Task guides for agents |
| `docs/decisions/` | One file per decision, with a searchable header |
| `.claude/rules/` | Symlinks to path-scoped task guides, loaded automatically by Claude Code |
| `.agents/skills/` | Codex skills: `simplify`, `security-review`, and pointer skills for path-scoped guides |
| `.github/pull_request_template.md` | Pull request description with the required review record |

## External dependencies

TODO(template): Services, APIs, fonts, and packages the project relies on at runtime, or "None".

## Deployment

TODO(template): Where it is hosted, what triggers a deploy, whether previews exist, and which files are excluded from what is served.

## Not yet decided

TODO(template): Open choices that agents must not decide on their own, such as framework, database, authentication, or hosting.
