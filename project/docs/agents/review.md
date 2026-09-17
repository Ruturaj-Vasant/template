# Review guide

Read this when a feature is finished, before anything merges into `main`, or when asked to review code.

## Markdown files

Markdown files are notes, and are never verified or reviewed (see "Definition of done" in `AGENTS.md`).
Every review below looks only at the branch's other files.
List them from the feature branch:

```bash
git diff --name-only main...HEAD -- . ':(exclude)*.md'
```

If it prints nothing, the branch skips verification and every review, and its review record says "Skipped: Markdown only".
Otherwise, discard any review finding about a Markdown file.

## Choosing the review steps

Reviews cost time, so a branch gets the steps its risk calls for, not all of them every time.
Pick the row that matches the branch's non-Markdown changes, then add the steps below the table that apply.
When unsure between two rows, pick the higher one.

| The branch... | Steps |
|---|---|
| Changes only Markdown files | None, and no verification |
| Fixes a bug or adjusts existing behavior, with no new capability | Code review |
| Adds a feature or capability, or restructures code | Simplify, code review |

Also run, whichever row applies:

- **Security review** when the changes touch authentication, permissions, secrets or tokens, input from users or outside systems, file handling, dependencies, build or deployment configuration, removed ignore rules, or an area listed under "Project security checks".
- **Performance** when the changes touch database queries, network calls, work that grows with the amount of data (loops, lists, rendering many items), startup, or an area listed under "Project performance checks".

Verification runs for every branch that changes a file other than Markdown.
If a fix made during review moves the branch into another row or adds an area, run the steps that now apply.

## Pre-merge review

Commit your work first, so every tool reviews the same changes.
Run the chosen steps in this order, skipping the ones not chosen:

1. **Simplify** first, because it edits code.
2. **Performance** second, because it edits code too.
3. **Code review** third, so bugs introduced by either edit are caught.
4. **Security review** last, on the final code.

| Step | Claude Code | Codex |
|---|---|---|
| Simplify: applies cleanups | `/simplify` | `$simplify` |
| Performance: applies safe speedups, proposes larger ones | `/performance` | `$performance` |
| Code review: reports bugs | `/code-review main...HEAD` | `/review`, then choose the base branch `main` (terminal: `codex review --base main`) |
| Security review: reports vulnerabilities | `/security-review`, then the project checks below | `$security-review` (includes the project checks) |

After each step, fix what it found and commit.
Verify again whenever a step changed code, and once more at the end if the tree changed since the last passing run.
If fixes were substantial, run the code review again on the result.

## Getting the scope right

Before the first review step, run this from the feature branch:

```bash
git fetch origin main:main main:refs/remotes/origin/main && git remote set-head origin --auto
```

It updates local `main`, `origin/main`, and `origin/HEAD`, which the reviews depend on and which fresh or single-branch clones (common in remote sessions) lack.
It refuses to run while `main` is checked out, so run it from the feature branch.
If `main` is checked out in another worktree, it fails for the same reason: run `git fetch origin main:refs/remotes/origin/main && git remote set-head origin main` instead, and use `origin/main` wherever the review commands say `main`.

- Always give Claude's `/code-review` a target such as `main...HEAD` or a PR number. Without one it only sees unpushed and uncommitted work, and finds nothing once a branch is pushed.
- Claude's `/security-review` compares the branch with `origin/HEAD`, which the command above sets.
- Codex's `$simplify`, `$performance`, and `$security-review` review `main...HEAD` plus uncommitted changes, without Markdown files.
- If any review's git command fails, stop and fix the setup. A failed diff is never "nothing to review", and never goes into the review record as a clean result.

## Project security checks

Generic security reviews do not know this project's domain.
When the diff touches these areas, also check:

TODO(template): Three to six checks specific to this project, each as a bold name followed by what must hold. Typical subjects: tenant or account boundaries (one user can never read or change another's data), audit history that cannot be edited, AI features that take instructions from user content (prompt injection) or act without approval, sensitive data reaching logs or third parties, and uploaded files. Leave as "None yet" if the project has no such code.

## Project performance checks

Generic performance reviews do not know where this project spends its time.
When the diff touches these areas, also check:

TODO(template): The project's performance-critical areas, each as a bold name followed by what must hold, and how to measure it (a benchmark command, a profiler, a query plan check, a test that times an operation on synthetic data). Typical subjects: the queries behind the busiest screens and their indexes, lists that can grow without limit, work on the main thread or in request handlers, calls to slow external services or AI models, and startup. Leave as "None yet" if the project has no such code.

## Review record

Every pull request into `main` includes the review record table from `.github/pull_request_template.md`.
Every step not run says why (for example "Not required: bug fix with no security-sensitive area"), and every skipped finding needs a reason.
For a branch that changes only Markdown files, one line saying "Skipped: Markdown only" is enough.

## Scope check

- Every requirement in the task is implemented.
- Nothing outside the requested scope changed, and `git diff --stat main...HEAD` matches what you intended.
- No placeholder, debug, or commented-out code is left behind.
- The change does not contradict a current decision that touches the same paths. If it must, write a new decision that supersedes the old one.
- The project principles in `AGENTS.md` still hold.

## Acting on findings

Reviewers asked to find gaps usually find some, even when the work is sound.
Fix findings that affect correctness, security, or the stated requirements.
Treat style preferences and speculative hardening as optional.
Never accept a cleanup that breaks a project principle in `AGENTS.md`, or removes input validation, error handling, or accessibility, to make code shorter.
Record anything you chose not to fix, with the reason.

## Reviewing someone else's change

Report findings ranked most severe first.
For each, give the file and line, what goes wrong, and a concrete scenario that triggers it.
Do not report style preferences as defects.
