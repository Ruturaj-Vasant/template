# Adopting the template

Instructions for a coding agent asked to set up a project from this template, or to update a project's setup from it.
Do "Before any path", then the one path that matches the request.
Paths refer to shared sections by name.

## Before any path

1. Clone the template outside the project folder, for example into a temporary directory: `git clone https://github.com/Ruturaj-Vasant/template.git <tmp>/template`. Never clone it inside the project.
2. Record the template commit you are using: `git -C <tmp>/template rev-parse HEAD`.
3. In the project, run `git status` and preserve any uncommitted work.
4. Create a branch in the project, for example `setup/agent-docs`. Never do this work on `main`.
5. For Path 1 or 2, read "What the system is" in the template's `README.md`. Read files in `project/` as you copy or fill them, not all up front.

Everything under `project/` maps to the same path at the project root: `project/docs/agents/review.md` becomes `docs/agents/review.md`.

## Path 1: new project

Use this when the project has no agent instructions yet (no `AGENTS.md`, `CLAUDE.md`, or similar rule files).

1. Check that copying overwrites nothing: `(cd <tmp>/template/project && find . -type f) | while read -r f; do [ -e "<project>/$f" ] && echo "exists: $f"; done` must print nothing. If it prints anything, follow Path 2 instead.
2. Copy the contents of `project/` into the project's root: `cp -R <tmp>/template/project/. <project>/`.
3. Do "Setting up the copied files". It fills the project's principles in `AGENTS.md`, what is known about the product in `docs/PRODUCT.md`, and what is not decided yet under "Still open" in `docs/ARCHITECTURE.md`.
4. Do "Finishing every path".

## Path 2: existing project

Use this when the project already has agent instructions, decision notes, or review habits.
The goal is the template's structure with none of the project's existing rules lost.

1. Read every existing instruction file first: `AGENTS.md`, `CLAUDE.md`, nested copies, `.cursorrules`, `.github/copilot-instructions.md`, `.claude/rules/`, and any decision or architecture notes.
2. Never overwrite an existing file with the template's copy. Merge instead:
   - Rules that apply everywhere go into the matching section of `AGENTS.md`.
   - Detailed rules for one kind of task go into a guide in `docs/agents/`, routed from the task guides table.
   - Project facts (what exists, paths, deployment) go into `docs/SYSTEMS.md`.
   - Product strategy, and the reasons for it, go into `docs/PRODUCT.md`.
   - Architecture choices (vendors, languages, frameworks, overall design), with their reasons and rejected options, go into "Choices and why" in `docs/ARCHITECTURE.md`.
   - Reasons behind working rules go into `docs/agents/background.md`.
   - Reasons for how a particular feature was built go into decision files.
3. Keep every existing rule, reworded only where needed to fit, unless the user explicitly agrees to drop it.
4. If `CLAUDE.md` and `AGENTS.md` both exist with different content, merge them into `AGENTS.md`; "Setting up the copied files" then makes `CLAUDE.md` a link.
5. Sort old decision notes, and existing decision files, as in step 2. Only feature decisions stay as decision files, in the format in `docs/decisions/README.md`.
   Keep their original dates, and number them in date order. Keep an existing numbering scheme if its IDs are already cited in code, and never reuse a retired number.
   Say in `docs/decisions/README.md` which numbers were retired and where their content went.
6. Copy the template files the project does not have yet, such as the review skills (including `performance`), the pull request template, and the guides.
7. Do "Setting up the copied files".
8. Do "Finishing every path". In the pull request, also list every existing rule and decision and where it went, and anything you were not sure where to put.

## Path 3: update from the template

Use this when the project already adopted the template and the user asks to update its setup.

1. Find the adopted template commit: `rg -n "^Template commit:" docs/`.
   If `docs/agents/background.md` has it, use that one.
   Projects adopted before the "Decisions are for features only" change have it only in an adoption decision; if more than one decision matches, use the one no other decision lists in `supersedes:`.
   If nothing matches, the project predates this record: follow Path 2 instead.
2. List what changed since then: `git -C <tmp>/template log --oneline <old-commit>..HEAD` and `git -C <tmp>/template diff <old-commit>..HEAD -- project/ CHANGELOG.md`.
   The `CHANGELOG.md` part of the diff says what adopted projects must do.
3. Apply only those changes. The project has customized its copies, so apply each change by intent, not by pasting whole files.
4. If a template change conflicts with a project rule or a current decision, keep the project's version and list the conflict in the pull request.
5. Do "Recording the template commit" with the new commit. In the pull request, summarize the template changes applied.
6. Do "Finishing every path".

## Setting up the copied files

Do these in order, so no placeholder is filled in a file that is then deleted.

1. **Links.** The template ships no symlinks, so create them: `ln -s AGENTS.md CLAUDE.md`; the performance skill for Claude Code: `mkdir -p .claude/skills/performance && ln -s ../../../.agents/skills/performance/SKILL.md .claude/skills/performance/SKILL.md`; and one link per path-scoped guide: `mkdir -p .claude/rules && ln -s ../../docs/agents/<guide>.md .claude/rules/<guide>.md`.
2. **Example guide.** Rename `docs/agents/example-guide.md` and `.agents/skills/example-guide/` to a real guide (for example `frontend`), including the `name:` in the skill, and link it as above. If the project has no path-scoped guide yet, delete both and the first row of the task guides table.
3. **Graphify.** If the project does not use Graphify, delete `docs/agents/graphify.md`, its row in the task guides table, and the Graphify bullet in `AGENTS.md`.
4. **End-to-end checks.** Fill the placeholders in `docs/agents/end-to-end.md` with the project's own commands, and delete the section that does not apply (a project with no browser deletes "In a browser"; one that ships nothing for Apple platforms deletes "On Apple platforms"). If the project has no user-facing surface at all, delete the guide and its row in the task guides table.
5. **Default branch.** If it is not `main`, find every mention with `rg -n "\bmain\b" AGENTS.md docs .agents .github` and replace them.
6. **Placeholders.** Fill every placeholder that `rg -n --hidden "TODO\(template\)"` lists, until it prints nothing. The `Template commit:` line is filled as "Recording the template commit" says. Never invent facts to fill one: ask the user, or record what is unknown, under "Still open" in `docs/ARCHITECTURE.md` or as "Unknown" in `docs/PRODUCT.md`.
7. **Deployment.** If the repository is served as static files (for example without a build step), exclude `AGENTS.md`, `CLAUDE.md`, `docs`, `.claude`, `.agents`, and `.github` from what is served.
8. **Check links.** `find . -path ./.git -prune -o -type l ! -exec test -e {} \; -print` prints nothing.

## Finishing every path

1. Run the header checks in `docs/decisions/README.md`.
2. Commit and push the branch, then run `git remote set-head origin --auto`, so `/security-review` works even when run on its own.
3. Run the review steps `docs/agents/review.md` chooses, unless the adoption changed only Markdown files (see "Markdown files" there).
4. Open a pull request with the review record.
5. Report: the path followed and the template commit used; placeholders filled and anything left unknown; existing rules moved and where; verification and header check results; anything you were unsure about.

## Recording the template commit

Every path records the template commit, so the next update knows where to start.
Setting up is not a feature, so it is not recorded as a decision.

Put the full 40-character hash from "Before any path" step 2 on the `Template commit:` line near the top of `docs/agents/background.md`, replacing any earlier hash:

```markdown
Template commit: <full commit hash>
```

If an older adoption decision holds the previous hash, leave that file as it is and say in the pull request that the record moved.
