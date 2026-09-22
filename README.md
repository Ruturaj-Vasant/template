# Project setup template

A documentation and review setup for projects built with coding agents (Claude Code and Codex).
It gives every session, local or remote, the same rules, the right context for the task, and a review habit before anything merges.

## What the system is

- **One instruction file.** `AGENTS.md` holds the standing rules; `CLAUDE.md` is a symlink to it, so Claude Code and Codex read the same thing.
- **Task guides.** Detailed rules live in `docs/agents/` and are read only when the task needs them. Path-scoped guides load automatically: through `.claude/rules/` symlinks in Claude Code and pointer skills in `.agents/skills/` in Codex.
- **Current state.** `docs/SYSTEMS.md` records what exists now.
- **Decisions.** `docs/decisions/` holds one file per decision, with a one-line header that agents search instead of reading every file.
- **Pre-merge review.** Before a branch merges into `main`, it gets the review steps its risk calls for (simplify, performance, code review, security review, end-to-end), and the results go in the pull request.
  Anything a browser or an Apple device decides is driven by a real tool: Playwright for the browser, Xcode and Swift for Apple platforms.
  Markdown files are notes and are never verified or reviewed.
- **Code graph (optional).** Graphify in code-only mode for architecture and change-impact questions.

## What is in this repository

| Path | Purpose |
|---|---|
| `project/` | The files copied into a project. Placeholders are marked `TODO(template)`. |
| `ADOPT.md` | Step-by-step instructions for an agent: new project, existing project, or update from the template. |
| `CHANGELOG.md` | What changed in the template, and what an adopted project must do to take the change. |
| `AGENTS.md` | Rules for agents working on the template itself. |

The files to copy live in `project/` rather than at the root, and `project/` ships no `CLAUDE.md` or symlinks, so an agent working on this repository never loads placeholder rules as its own instructions.

## How to use it

Tell the agent, from inside the project:

> Set this project up using github.com/Ruturaj-Vasant/template, follow ADOPT.md.

Because the repository is public, remote sessions can read it without logging in.
The agent clones it outside the project folder and follows one of the paths in `ADOPT.md`, which has the full steps; the lists below are a summary.

**New project:**

1. Copy `project/` into the project's root.
2. Fill every `TODO(template)`.
3. Write D-001 recording the template commit and the project's principles.
4. Set `origin/HEAD`.

**Existing project:**

1. Work on a branch.
2. Keep every existing rule, moving each one to where it belongs, and never overwrite.
3. Turn old decision notes into decision files.
4. List anything it was not sure about.
5. Run the pre-merge review if anything other than Markdown files changed.

Either way, the project records which template commit it adopted in a decision.

**Updating a project later:**

> Update this project's setup from github.com/Ruturaj-Vasant/template, follow ADOPT.md.

The agent finds the recorded template commit, lists what changed in the template since then, and applies only that.
Without the recorded commit, every update means comparing everything by hand.

## Improving the template

When a project teaches you something generic, such as a better review step, bring it back here through a pull request.
Add a `CHANGELOG.md` entry saying what adopted projects must do to take the change.
Keep project-specific content, personal details, and anything private out: this repository is public.
