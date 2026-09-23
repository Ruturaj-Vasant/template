# Working agreement

Standing instructions for coding agents working on this template repository.
`CLAUDE.md` is a symlink to this file: edit `AGENTS.md`, and never replace the symlink with a copy.

## What this repository is

A template that other projects copy.
`project/` holds the files that get copied into a project's root.
`README.md` explains the system to people, `ADOPT.md` tells agents how to apply it, and `CHANGELOG.md` tells adopted projects what changed.
The files in `project/` are not instructions for this repository; they are the product.
`project/` ships no `CLAUDE.md` or other symlinks, so its placeholder rules are never loaded here; `ADOPT.md` creates the links in each project.
Work from the repository root.

## Rules

- Changes reach `main` only through a pull request from a branch.
- Commit every change as soon as it is made, and push the branch whenever the repository has a remote, even when the work is unfinished or may never be merged. A commit saves progress; merging is a separate choice. Never end a session with work only in the working tree.
- Never credit an AI tool as an author or contributor: no `Co-Authored-By` trailers, "Generated with" lines, or similar attribution for Claude, Codex, or any other agent in commits, pull requests, code, or documentation.
- This repository is public. Never add project-specific content, personal details, real data, secrets, or references to specific private repositories.
- Keep `project/` generic. Anything a project must decide for itself is a placeholder marked `TODO(template): <what to fill in>`.
- `project/` must stay internally consistent: every path, section name, and command one file refers to must exist in another. Check this after every change.
- Never add symlinks or a `CLAUDE.md` under `project/`.
- Write prose one sentence per line. State each rule in one place and link to it from elsewhere.
- Write for whoever reads the file. `README.md`, `ADOPT.md`, and `CHANGELOG.md` are read by people, so they use plain, direct English with short sentences and ordinary words. Files that mainly instruct an agent are terse and structured. A file both read is written plainly.
- Do not remove or weaken a rule in `project/` without the user's explicit permission.

## Changing the template

1. Make the change on a branch.
2. If an adopted project must do something to take the change, add an entry at the top of `CHANGELOG.md`.
3. If the change affects how projects adopt or update, update `ADOPT.md` and `README.md` to match.
4. Check `project/` consistency: `rg -n --hidden "TODO\(template\)" project/` lists only intended placeholders, `find project -type l` prints nothing, and skill front matter parses.
5. Run the review steps `project/docs/agents/review.md` chooses, and put the review record in the pull request, unless the change touches only Markdown files.
   Neither Claude Code nor Codex loads the skills under `project/` here; ask the agent to follow `project/.agents/skills/<name>/SKILL.md` directly.
