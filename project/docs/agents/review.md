# Review guide

Read this when a feature is finished, before anything merges into `main`, or when asked to review code.

## Pre-merge review

Every branch gets three reviews before it merges into `main`.
Commit your work first, so every tool reviews the same changes.
Run the reviews in this order:

1. **Simplify** first, because it edits code.
2. **Code review** second, so bugs introduced by the cleanup are caught too.
3. **Security review** last, on the final code.

| Step | Claude Code | Codex |
|---|---|---|
| 1. Simplify: applies cleanups | `/simplify` | `$simplify` |
| 2. Code review: reports bugs | `/code-review main...HEAD` | `/review`, then choose the base branch `main` (terminal: `codex review --base main`) |
| 3. Security review: reports vulnerabilities | `/security-review`, then the project checks below | `$security-review` (includes the project checks) |

After each step, fix what it found and commit.
Verify again whenever a step changed files, and once more at the end.
If fixes were substantial, run the code review again on the result.
Security review may be skipped only when the branch changes nothing but Markdown files; say so in the review record.

## Getting the scope right

Before the first review step, run this from the feature branch:

```bash
git fetch origin main:main main:refs/remotes/origin/main && git remote set-head origin --auto
```

It brings local `main`, `origin/main`, and `origin/HEAD` up to date, which every review command below depends on.
A fresh or single-branch clone, common in remote sessions, has none of them, and the reviews would otherwise fail or compare against a stale `main`.
It refuses to run while `main` itself is checked out, which is fine because reviews run on feature branches.

- Always give Claude's `/code-review` a target such as `main...HEAD` or a PR number. Without one it only sees unpushed and uncommitted work, and finds nothing once a branch is pushed.
- Claude's `/security-review` compares the branch with `origin/HEAD`, which the command above sets.
- Codex's `$simplify` and `$security-review` review `main...HEAD` plus uncommitted changes.
- If any review's git command fails, stop and fix the setup. A failed diff is never "nothing to review", and never goes into the review record as a clean result.

## Project security checks

Generic security reviews do not know this project's domain.
When the diff touches these areas, also check:

TODO(template): Three to six checks specific to this project, each as a bold name followed by what must hold. Typical subjects: tenant or account boundaries (one user can never read or change another's data), audit history that cannot be edited, AI features that take instructions from user content (prompt injection) or act without approval, sensitive data reaching logs or third parties, and uploaded files. Leave as "None yet" if the project has no such code.

## Review record

Every pull request into `main` includes the review record table from `.github/pull_request_template.md`.
Every skipped finding or step needs a reason.

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
Never accept a cleanup that removes an audit trail, units, an approval step, input validation, error handling, or accessibility to make code shorter.
Record anything you chose not to fix, with the reason.

## Reviewing someone else's change

Report findings ranked most severe first.
For each, give the file and line, what goes wrong, and a concrete scenario that triggers it.
Do not report style preferences as defects.
