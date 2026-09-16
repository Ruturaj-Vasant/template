# Changelog

What changed in the template, newest first.
Each entry says what an adopted project must do to take the change.
Projects find their starting point with `rg "^Template commit:" docs/decisions/D-*` and apply the entries added after that commit (see `ADOPT.md`, Path 3).

## Initial template

- `AGENTS.md` working agreement with `CLAUDE.md` as a symlink.
- Task guides in `docs/agents/`: review, handoff, Graphify, and an example path-scoped guide with its Claude Code link and Codex pointer skill.
- `docs/SYSTEMS.md` for current state and `docs/decisions/` for one-file-per-decision records with searchable headers and header checks.
- Pre-merge review (simplify, code review, security review) for Claude Code and Codex, with Codex skills in `.agents/skills/` and a review record in `.github/pull_request_template.md`.

**Adopted projects must:** nothing; this is the starting point.
