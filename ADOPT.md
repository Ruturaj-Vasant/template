# Adopting the template

Instructions for a coding agent asked to set up a project from this template, or to update a project's setup from it.
Follow the path that matches the request.

## Before any path

1. Clone the template outside the project folder, for example into a temporary directory: `git clone https://github.com/Ruturaj-Vasant/template.git <tmp>/template`. Never clone it inside the project.
2. Record the template commit you are using: `git -C <tmp>/template rev-parse HEAD`.
3. In the project, run `git status` and preserve any uncommitted work.
4. Create a branch in the project, for example `setup/agent-docs`. Never do this work on `main`.
5. Read `README.md` in the template to understand the system, then the files in `project/` you are about to copy.

Everything under `project/` maps to the same path at the project root: `project/docs/agents/review.md` becomes `docs/agents/review.md`.
Copy with a method that keeps symlinks as symlinks (`cp -R`, or `rsync -a`), and check afterwards that `CLAUDE.md` and `.claude/rules/*` are still links.

## Path 1: new project

Use this when the project has no agent instructions yet (no `AGENTS.md`, `CLAUDE.md`, or similar rule files).

1. Copy the contents of `project/` into the project's root.
2. Fill every placeholder. `rg -n "TODO\(template\)"` lists them; the path is done when it prints nothing.
   Never invent facts to fill a placeholder: ask the user, or write what is unknown in `docs/SYSTEMS.md` under "Not yet decided".
3. Rename the example guide to a real one (for example `frontend`), or delete all three example files if the project has no path-scoped guide yet:
   `docs/agents/example-guide.md`, `.claude/rules/example-guide.md`, and `.agents/skills/example-guide/`.
4. If the project does not use Graphify, delete `docs/agents/graphify.md`, its row in the task guides table, and the Graphify section of `AGENTS.md`.
5. If the default branch is not `main`, replace `main` in `AGENTS.md`, `docs/agents/review.md`, and both review skills.
6. Write `docs/decisions/D-001-agent-setup-from-template.md` as described in "The adoption decision" below.
7. Fill `docs/SYSTEMS.md` with what exists now.
8. If the repository is served as static files (for example Vercel or GitHub Pages without a build step), exclude the agent files from the deployment: `AGENTS.md`, `CLAUDE.md`, `docs`, `.claude`, `.agents`, `.github`.
9. Run the header checks in `docs/decisions/README.md`.
10. Push the branch, then set `origin/HEAD` so the review tools work: `git remote set-head origin --auto`.
11. Run the pre-merge review in `docs/agents/review.md` and open a pull request with the review record.

## Path 2: existing project

Use this when the project already has agent instructions, decision notes, or review habits.
The goal is the template's structure with none of the project's existing rules lost.

1. Read every existing instruction file first: `AGENTS.md`, `CLAUDE.md`, nested copies, `.cursorrules`, `.github/copilot-instructions.md`, `.claude/rules/`, and any decision or architecture notes.
2. Never overwrite an existing file with the template's copy. Merge instead:
   - Rules that apply everywhere go into the matching section of `AGENTS.md`.
   - Detailed rules for one kind of task go into a guide in `docs/agents/`, routed from the task guides table.
   - Project facts (what exists, paths, deployment) go into `docs/SYSTEMS.md`.
   - Reasons for past choices go into decision files.
3. Keep every existing rule, reworded only where needed to fit, unless the user explicitly agrees to drop it.
4. If `CLAUDE.md` and `AGENTS.md` both exist with different content, merge them into `AGENTS.md`, then replace `CLAUDE.md` with a symlink to it.
5. Turn old decision notes into decision files with the header format in `docs/decisions/README.md`.
   Keep their original dates, and number them in date order. Keep an existing numbering scheme if one exists and its IDs are already cited in code.
6. Add the parts the project does not have: the pre-merge review, the review skills, the pull request template, and path-scoped guide links.
7. Follow steps 2 to 5 and 7 to 10 of Path 1 for the parts you copied.
8. Write the adoption decision with the next free number.
9. In the pull request, list every existing rule and where it went, and anything you were not sure where to put.
10. Run the pre-merge review in `docs/agents/review.md` and put the review record in the pull request.

## Path 3: update from the template

Use this when the project already adopted the template and the user asks to update its setup.

1. Find the adopted template commit: `rg -n "^Template commit:" docs/decisions/D-*`.
   If more than one decision matches, use the one no other decision lists in `supersedes:`.
   If none matches, the project predates this record: treat the update as Path 2.
2. In the template clone, list what changed since then:
   - `git -C <tmp>/template log --oneline <old-commit>..HEAD`
   - `git -C <tmp>/template diff <old-commit>..HEAD -- project/`
   - The entries in `CHANGELOG.md` added after that commit, which say what adopted projects must do.
3. Apply only those changes. The project has customized its copies, so apply each change by intent, not by pasting whole files.
4. If a template change conflicts with a project rule or a current decision, keep the project's version and list the conflict in the pull request.
5. Write a new adoption decision that supersedes the previous one and records the new template commit.
6. Run the header checks, then the pre-merge review, and open a pull request.

## The adoption decision

Every path ends with a decision recording the template commit, so the next update knows where to start.

```markdown
---
status: Accepted
date: YYYY-MM-DD
areas: agents, tooling
touches: AGENTS.md, CLAUDE.md, docs/agents/, docs/decisions/, .claude/rules/, .agents/skills/, .github/pull_request_template.md
supersedes: none
decision: "Agent instructions, task guides, decisions, and the pre-merge review follow the project setup template."
---

# D-NNN: Agent setup from the project template

Template commit: <full 40-character commit hash>

## Context

## Decision

## Alternatives rejected

## Consequences

## Verification
```

- `Template commit:` starts its own line, so `rg "^Template commit:"` finds it.
- In Path 1, the Decision section also states the project's principles and what is not decided yet.
- In Path 2, the Consequences section lists which existing rules moved and where.
- In Path 3, `supersedes:` names the previous adoption decision, and the Context section summarizes the template changes applied.
- Keep `touches:` to paths that exist in the project, so the header checks pass.

## Report

Finish with:

- The path followed and the template commit used.
- Every `TODO(template)` filled, and anything left as unknown.
- Existing rules moved or merged, and where they went.
- The verification and header check results.
- Anything you were unsure about.
