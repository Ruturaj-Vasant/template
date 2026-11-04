---
summary: "What exists in the project right now: the files, the parts, the outside services it relies on, and deployment."
touches: TODO(template): the main code folders and deployment files, comma separated, or none
---

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
| `docs/product/` | Product areas, one file each with a searchable header |
| `docs/ARCHITECTURE.md` | Living architecture plan: the parts, where each runs, and the reasons for each choice |
| `docs/agents/` | Task guides for agents, `guide-outlines.md` for the guides not written yet, and `background.md` with the reasons behind the working rules |
| `docs/decisions/` | Feature decisions, one file each with a searchable header |
| `docs/design/` | The chosen design direction and its mockups |
| `.claude/rules/` | Symlinks to path-scoped task guides, loaded automatically by Claude Code |
| `.agents/skills/` | Codex skills: `simplify`, `performance`, `ui-check`, `flowcheck`, `security-review`, `ux-design`, and pointer skills for path-scoped guides |
| `.claude/skills/` | Symlinks to the skills Claude Code has no built-in version of: `performance`, `ui-check`, `flowcheck`, and `ux-design` |
| `.github/pull_request_template.md` | Pull request description with the required review record |

## External dependencies

TODO(template): Services, APIs, fonts, and packages the project relies on at runtime, or "None".

## Deployment

TODO(template): Where it is hosted, what triggers a deploy, whether previews exist, and which files are excluded from what is served.

## Not yet decided

Open choices are listed under "Still open" in `docs/ARCHITECTURE.md`, and agents never decide them on their own.
