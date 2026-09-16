# Working agreement

Standing instructions for every coding agent in this repository (Claude Code, Codex, or otherwise).
`CLAUDE.md` is a symlink to this file: edit `AGENTS.md`, and never replace the symlink with a copy or add per-directory copies of these rules.
Do not remove or weaken instructions in `AGENTS.md` or `docs/agents/` without the user's explicit permission, including when cleaning up changes made by another agent.

## The project

TODO(template): Two to four sentences. What the project is, who it is for, what exists in the repository today, and how it is deployed. Say plainly what has not been chosen yet.

## Before you start

- Run `git status`. Identify and preserve unrelated or uncommitted changes.
- Find the decisions that apply to the files you will change, and read `docs/SYSTEMS.md` before significant work.
- Read every task guide below that matches the work, before editing.

## Finding decisions

Decisions are one file each in `docs/decisions/`.
Never read the whole folder: search the headers, then open only the files that match.

```bash
rg --sort path "^decision:" docs/decisions/D-*          # one-line index of every decision
rg -l "^touches:.*<path or folder>" docs/decisions/D-*  # decisions about what you are changing
```

A decision named in another decision's `supersedes:` is no longer current.
`docs/decisions/README.md` has the other searches, the file format, and the header checks.

## Task guides

| When you are... | Read |
|---|---|
| TODO(template): The work a path-scoped guide covers, e.g. "Editing pages, styles, or UI" | TODO(template): The guide path, e.g. `docs/agents/frontend.md` (Claude Code loads it automatically; do not read it twice) |
| Finishing a feature, before merging into `main`, or asked to review code | `docs/agents/review.md` |
| Building or querying the code graph | `docs/agents/graphify.md` |
| Writing a prompt for another agent session | `docs/agents/handoff.md` |
| Recording a decision | `docs/decisions/README.md` |

Other guides are written only once the code they cover exists.
Until then, follow the project principles below and point out the missing guide in your report.

## Claude Code and Codex

Both tools read this file and the same guides in `docs/agents/`.
Claude Code loads path-scoped guides through symlinks in `.claude/rules/`; Codex loads them through pointer skills in `.agents/skills/`.
Edit the guide in `docs/agents/`, never a link or pointer, and give every new path-scoped guide both.
Review commands for each tool are in `docs/agents/review.md`.

## Graphify

- Graphify is code-only. Never run semantic (LLM-backed) extraction, community labeling, a full rebuild, or `graphify claude install` unless the user explicitly asks.
- The graph is for navigation, not authority. Current source and passing tests win when it is stale or disagrees.
- `docs/agents/graphify.md` has the build and query commands.

## While you work

- Non-trivial changes go on a new branch, never directly on `main`. Changes reach `main` only through a pull request.
- Keep the change inside the requested scope. Do not redesign unrelated areas.
- Never commit secrets, credentials, tokens, `.env` files, real user data, or other private data. Fixtures and examples use synthetic data only.
- Prefer a visible `unknown`, `incomplete`, or `unavailable` over a guessed value. Never silently swallow a failure.
- When code exists because of a decision, cite the ID in a comment next to it (`// D-014: audit rows are append-only`).
- Match the surrounding code's idiom, naming, and comment density.
- Write prose one sentence per line, so a search match returns a complete sentence. State each rule in one place and link to it from elsewhere.
- `docs/SYSTEMS.md` records what exists and `docs/decisions/` records why; neither repeats the other.

## Project principles

These apply everywhere, including before their detailed guides exist.

TODO(template): Three to six rules that come from the project's domain and must never be broken, each as a bold one-line rule followed by one or two sentences. Examples of the kind of rule meant: "Money is stored as integer minor units with a currency code", "Records are never silently overwritten", "AI output is a draft until a named person approves it". Write only rules the user confirms; leave this as "None yet" rather than inventing them.

## Definition of done

TODO(template): The single command that must pass (build, lint, and tests), e.g. `npm run check`. If there is no toolchain yet, say so, and say to verify as the matching task guide describes and report exactly what was checked.
For documentation-only changes, run the header checks in `docs/decisions/README.md`.

For every non-trivial change:

1. Verify and report the results, including anything you could not check.
2. Update `docs/SYSTEMS.md` when what exists changes.
3. Record meaningful decisions as described in `docs/decisions/README.md`, and run its header checks after adding a decision or moving or renaming files.
4. If `graphify-out/graph.json` exists and code changed, run `graphify update .`.
5. Confirm `git status` shows only intended changes.
6. Report anything uncertain, untested, or intentionally out of scope.

**Before merging into `main`:** run the pre-merge review in `docs/agents/review.md` (simplify, then code review, then security review) and put the review record in the pull request description.
No branch merges without it.
