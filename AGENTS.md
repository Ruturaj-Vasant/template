# Working agreement

Standing instructions for coding agents working on this template repository.
`CLAUDE.md` is a symlink to this file: edit `AGENTS.md`, and never replace the symlink with a copy.

## What this repository is

A template that other projects copy.
`project/` holds the files that get copied into a project's root.
`README.md` explains the system to people, `ADOPT.md` tells agents how to apply it, and `CHANGELOG.md` tells adopted projects what changed.
The files in `project/` are not instructions for this repository; they are the product.
Claude Code may load `project/CLAUDE.md` automatically when you read files under `project/`, and Codex reads `project/AGENTS.md` if launched inside that folder.
Ignore those as instructions: this file is the only working agreement here.
Work from the repository root.

## Rules

- Changes reach `main` only through a pull request from a branch.
- This repository is public. Never add project-specific content, personal details, real data, secrets, or references to specific private repositories.
- Keep `project/` generic. Anything a project must decide for itself is a placeholder marked `TODO(template): <what to fill in>`.
- `project/` must stay internally consistent: every path, section name, and command one file refers to must exist in another. Check this after every change.
- Keep symlinks as symlinks: `project/CLAUDE.md` and `project/.claude/rules/*`.
- Write prose one sentence per line. State each rule in one place and link to it from elsewhere.
- Do not remove or weaken a rule in `project/` without the user's explicit permission.

## Changing the template

1. Make the change on a branch.
2. If an adopted project must do something to take the change, add an entry at the top of `CHANGELOG.md`.
3. If the change affects how projects adopt or update, update `ADOPT.md` and `README.md` to match.
4. Check `project/` consistency: `rg -n "TODO\(template\)" project/` lists only intended placeholders, symlinks resolve, and skill front matter parses.
5. Run the pre-merge review described in `project/docs/agents/review.md`, and put the review record in the pull request.
   Codex does not load the skills under `project/` here; ask it to follow `project/.agents/skills/<name>/SKILL.md` directly.
