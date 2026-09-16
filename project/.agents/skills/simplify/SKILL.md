---
name: simplify
description: Clean up the code changed on the current branch for reuse, simplicity, efficiency, and placement, then apply the fixes. Use as step 1 of the pre-merge review, or when asked to simplify changes. Does not hunt for bugs; use /review for that.
---

# Simplify

You are improving the quality of the changed code, not hunting for bugs.
Review it, fix what you find, and report what you changed.

## Scope

1. Run the setup command from "Getting the scope right" in `docs/agents/review.md`, so `main` exists and is current.
2. Run `git diff main...HEAD` and `git diff HEAD` to collect committed and uncommitted changes on this branch. Use `origin/main` instead of `main` if you used the worktree fallback in that section.
3. If a git command fails, stop and report the error. Never report "nothing to simplify" after a failed command.
4. If both diffs are empty, stop and say there is nothing to simplify.
5. Only change code inside that diff, plus the smallest edits needed elsewhere to reuse something that already exists.

## Review the diff from four angles

Work through every angle; do not skip one.

- **Reuse.** Does the change reimplement something that already exists in this repository, the language's standard library, the browser or platform, or an installed dependency? Replace it with the existing thing.
- **Simplicity.** Look for dead code, duplicated logic, needless abstraction, wrappers that add nothing, options nobody uses, and code written for hypothetical future needs.
- **Efficiency.** Look for repeated work, redundant requests or queries, unnecessary loops, and data loaded but never used.
- **Placement.** Is each piece of logic in the right layer and file? Prefer fixing a problem where it originates over patching its symptoms downstream.

For each finding, note the file, the line, a one-line summary, and the concrete cost (what is duplicated, wasted, or harder to maintain).

## Apply the fixes

- Merge findings that point at the same line or mechanism, then fix each one.
- Skip any fix that would change intended behavior, reach well outside the diff, or that you judge to be a false positive. Note the skip instead of forcing it.
- Never make a cleanup that the "Acting on findings" section of `docs/agents/review.md` forbids.
- Follow `AGENTS.md` and any task guide that matches the files you touch.

## Finish

Run the verification described in `AGENTS.md`.
Summarize what was fixed, what was skipped and why, or confirm the code was already clean.
