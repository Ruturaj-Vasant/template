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
rg -l "^touches:.*<path or folder>" docs/decisions/D-*  # decisions about what you are changing; also search its parent folders
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
A path-scoped guide lives in `docs/agents/` and needs a symlink in `.claude/rules/` (Claude Code) and a pointer skill in `.agents/skills/` (Codex). Edit only the guide.

## While you work

- Non-trivial changes go on a new branch, never directly on `main`. Changes reach `main` only through a pull request.
- Keep the change inside the requested scope. Do not redesign unrelated areas.
- Never credit an AI tool as an author or contributor: no `Co-Authored-By` trailers, "Generated with" lines, or similar attribution for Claude, Codex, or any other agent in commits, pull requests, code, or documentation.
- Never commit secrets, credentials, tokens, `.env` files, real user data, or other private data. Fixtures and examples use synthetic data only.
- Prefer a visible `unknown`, `incomplete`, or `unavailable` over a guessed value. Never silently swallow a failure.
- When code exists because of a decision, cite the ID in a comment next to it (`// D-014: audit rows are append-only`).
- Match the surrounding code's idiom, naming, and comment density.
- Write prose one sentence per line, so a search match returns a complete sentence. State each rule in one place and link to it from elsewhere.
- Write for whoever reads the file. A document people read uses plain, direct English: short sentences, ordinary words, and any term explained the first time it appears. A file that mainly instructs an agent is terse and structured. A file both read is written plainly, because plain words cost a machine nothing and jargon costs a person a lot.
- Speak to the user the same way: simple language, and explain a term rather than assuming it.
- `docs/SYSTEMS.md` records what exists and `docs/decisions/` records why; neither repeats the other.
- Graphify is code-only and advisory: never run semantic (LLM-backed) extraction, community labeling, a full rebuild, or `graphify claude install` unless the user explicitly asks, and trust current source over a stale graph.

## Project principles

These apply everywhere, including before their detailed guides exist.

TODO(template): Three to six rules that come from the project's domain and must never be broken, each as a bold one-line rule followed by one or two sentences. Examples of the kind of rule meant: "Money is stored as integer minor units with a currency code", "Records are never silently overwritten", "AI output is a draft until a named person approves it". Write only rules the user confirms; leave this as "None yet" rather than inventing them.

## Definition of done

TODO(template): The single command that must pass (build, lint, and tests), e.g. `npm run check`. If there is no toolchain yet, say so, and say to verify as the matching task guide describes and report exactly what was checked.

For every non-trivial change:

1. When a file other than Markdown changed, verify and report the results, including anything you could not check.
2. Update `docs/SYSTEMS.md` when what exists changes.
3. Record meaningful decisions as described in `docs/decisions/README.md`, and run its header checks after changing `docs/decisions/` or moving or renaming files.
4. If `graphify-out/graph.json` exists and code changed, run `graphify update .`.
5. Confirm `git status` shows only intended changes.
6. Report anything uncertain, untested, or intentionally out of scope.

**Markdown files are notes.**
Files ending in `.md` (including `AGENTS.md`, guides, decisions, and skills) are never verified or reviewed, even on a branch that also changes code: verification and every review step cover only the other files.

**Before merging into `main`:** run the review steps that `docs/agents/review.md` chooses for the branch's risk, in its order (simplify, performance, code review, security review), and put the review record in the pull request description.
A branch that changes only Markdown files records "Skipped: Markdown only" instead.
