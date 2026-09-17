---
name: performance
description: Find and fix measurable performance problems in the code changed on the current branch, without removing or changing features. Use in the pre-merge review when docs/agents/review.md calls for it, or when asked to make changed code faster.
---

# Performance

You are making the changed code do less wasted work, without changing what it does.
Fix clear, local problems, propose larger ones, and report both.

## Scope

1. Run the setup command from "Getting the scope right" in `docs/agents/review.md`, so `main` exists and is current.
2. Run `git diff main...HEAD -- . ':(exclude)*.md'` and `git diff HEAD -- . ':(exclude)*.md'` to collect committed and uncommitted changes, without Markdown files, which are never reviewed (see `AGENTS.md`). Use `origin/main` instead of `main` if you used the worktree fallback in that section.
3. If a git command fails, stop and report the error. Never report "nothing to improve" after a failed command.
4. If both diffs are empty, stop and say there is nothing to review.
5. Only change code inside that diff and the code it calls directly.

## Rules

- Behavior stays identical: the same results, ordering, error cases, and user-visible states.
- Never remove or weaken a feature, a test, error handling, input validation, accessibility, or a security boundary to gain speed.
- Fix only problems you can show, with a measurement or a concrete argument about how the cost grows (per item, per user, per request, per render, per keystroke). "Might be slow" is a proposal, not a fix.
- Prefer removing work over caching it. A cache is the last resort, because it adds stale-data and invalidation bugs.
- Every cache you add states its key, what invalidates it, its size limit, and what a stale read does. Never cache data that `AGENTS.md` forbids storing.
- Add no dependency, thread, or concurrency mechanism for speed unless the gain is measured.
- Apply every item in the "Project performance checks" section of `docs/agents/review.md`.

## What to look for

Highest usual payoff first.

1. **Work inside a loop that belongs outside it:** a query, request, file read, pattern compile, formatter, or parser created once per item.
2. **One query or request per item** where one batched call would do.
3. **Blocking the user interface:** disk, network, database, parsing, or other slow calls on the main thread or during rendering.
4. **Recomputing the same result** on every render, request, or keystroke when its inputs have not changed.
5. **Unbounded growth:** collections, caches, or buffers with no limit; loading a whole table, file, or result set when a page or stream would do.
6. **Database cost:** no index for a new query's filter or sort, columns selected but never used, many writes outside one transaction.
7. **Independent operations run one after another** that could run concurrently within existing limits.
8. **Wasteful data movement:** copying large values, decoding the same payload twice, fetching fields that are never read.
9. **Startup:** new work before the first screen or response that could run later or only when needed.

Skip micro-optimizations the compiler or runtime already does, and code that runs once or only on tiny inputs.

## Measuring

- Use the measuring tools named in "Project performance checks" in `docs/agents/review.md` first.
- For a timing or memory claim, measure before and after on the same input, and report both numbers.
- If you cannot measure, say what you would measure and label the finding "unmeasured".

## Apply the fixes

- For each finding, note the file, the line, the cost, how it grows, and the change.
- Apply the findings that are clear, local, and keep behavior identical.
- Leave larger changes as proposals for the user, with their evidence: a new cache layer, pagination, a schema or index change, a new concurrency design, or anything that reaches well outside the diff.
- Never make a change that the "Acting on findings" section of `docs/agents/review.md` forbids.
- Follow `AGENTS.md` and any task guide that matches the files you touch.

## Finish

Run the verification described in `AGENTS.md`.
Report what you changed (with before and after numbers where measured), what you proposed without applying, and what you skipped and why, or confirm you found nothing worth changing.
