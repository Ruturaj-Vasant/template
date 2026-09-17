# Changelog

What changed in the template, newest first.
Each entry says what an adopted project must do to take the change.
Projects apply the entries added after their recorded template commit (see `ADOPT.md`, Path 3).

## Documentation-only branches skip verification and the pre-merge review

- `docs/agents/review.md` gains a "Documentation-only changes" section: a branch that changes nothing but Markdown files and symlinks to them skips the verification command and all three reviews, and records "Skipped: documentation only".
- The final verification after the reviews runs only if the tree changed since the last passing run.
- `AGENTS.md` definition of done, the pull request template, `README.md`, and `ADOPT.md` point to that rule.

**Adopted projects must:** add the "Documentation-only changes" section to `docs/agents/review.md` and remove its old "Security review may be skipped only when..." sentence; update definition-of-done item 1 and the "Before merging" paragraph in `AGENTS.md`; and update the review-record comment in `.github/pull_request_template.md`.
If the project treats some non-Markdown files as documentation, or some Markdown files as code, say so in that section.

## Initial template

- `AGENTS.md` working agreement with `CLAUDE.md` as a symlink.
- Task guides in `docs/agents/`: review, handoff, Graphify, and an example path-scoped guide with its Codex pointer skill. Symlinks are created during adoption, not shipped.
- `docs/SYSTEMS.md` for current state and `docs/decisions/` for one-file-per-decision records with searchable headers and header checks.
- Pre-merge review (simplify, code review, security review) for Claude Code and Codex, with Codex skills in `.agents/skills/` and a review record in `.github/pull_request_template.md`.

**Adopted projects must:** nothing; this is the starting point.
