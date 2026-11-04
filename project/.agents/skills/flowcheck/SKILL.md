---
name: flowcheck
description: Test the app the way real people use it, as each kind of user, through whole journeys across pages, against a real server and database, and keep those journeys as automated flow tests. Use in the pre-merge review when docs/agents/review.md calls for it, after adding or changing a feature, or when asked to test flows, user journeys, or how features work together.
---

# Flow check

You are checking that a feature works for the people who use it, from the first step to the last, across every page where its facts appear.
Unit tests, browser checks, and the other review steps look at one piece at a time; this step looks at what they miss: whole journeys, several people acting on the same data, and data changed underneath the app.
You fix clear problems, propose larger ones, and leave every journey behind as an automated flow test, so the suite grows with each feature.

## Scope

1. Run the setup command from "Getting the scope right" in `docs/agents/review.md`, so `main` exists and is current.
2. List the changed files without Markdown (`git diff --name-only main...HEAD -- . ':(exclude)*.md'`, with `origin/main` if you used the worktree fallback there) and name the features they touch. If a git command fails, stop and report it. If no person can notice the changes, stop and say so.
3. Read the product area files in `docs/product/` for those features and for every feature that shares their data (find them with the searches in `docs/product/README.md`), the roles in `docs/PRODUCT.md`, and the decisions that touch the changed files.
4. From those, list the journeys: who (each role, someone given or refused the permission the feature needs, someone new, someone who left), what they want, each step, and every page where the result must then show.

## Setup

"Flow tests" in `docs/agents/end-to-end.md` says where the suite lives, how to run it, and how to start a server and app of your own.

- Work in your own worktree, never in the main folder (`AGENTS.md`, "While you work").
- **Your own database.** Copy the test database into one of your own, plus a second, clean copy to reset to. Never use or migrate a database that another session's server uses. Copy from a database at or below your branch's migrations, never above: older code can refuse rows that newer code wrote.
- **Your own server and app**, on free ports, pointed at your copy. Local configuration made for this is never committed.
- **Test accounts.** Use only made-up test people. Read their logins in scripts at run time; never print, paste, or commit them. Make a role you have no login for by changing a test person's role in your own copy only.
- **Shared services.** Never write to a shared service whose records cannot be removed, such as a sign-in directory. Steps that need one, such as an invitation or a first sign-in, go under "could not test", for a person to try by hand.
- **Changing data directly.** Only in your own copy, and through whatever the project uses to record who made a change, so history stays complete.

## What to check

1. **Journeys, as each kind of user.** Each step works and says what happens next; the person can finish what they came to do without help; nothing is lost on the way.
2. **Every page agrees.** After each action, every page that shows the same fact shows the same thing. A count on one page and a list on another must add up.
3. **Who may do what.** For each role and permission: what is shown and hidden, and, by calling the server directly with that person's sign-in, that the server refuses what the screen hides. Nobody sees another account's or tenant's data, or anything their role keeps from them.
4. **People acting on the same data.** One person asks and another approves; one offers and another accepts; someone leaves while still holding work; two people change the same record at once, in two browsers.
5. **The data underneath.** States real users reach: overlapping dates, a person who left, an owner who left, something due today, something with no date, very long names, accented names, an empty account, hundreds of records.
6. **Time.** The server's day and the person's day differ for part of every day. Check what each page and each new record says in the person's evening and at midnight, in the time zones the project's users are in.
7. **Everywhere.** Each step at the screen widths the frontend guide names, by keyboard alone, with the server stopped mid-journey, with the browser's Back and a reload, and with history recorded for every change, if the project keeps history.

## Automate the journeys

- Every journey you test becomes a test in the flow suite, one file per journey, named in plain words so a failure reads as a sentence: "A manager approves time off: the person's turn moves to someone else".
- Flow tests run against the real server and database, unlike browser checks that answer the app with made-up responses.
- Add the new feature's journeys; never delete or weaken an existing journey. When a fix changes what a journey should do, change its test to the new behaviour and say why.
- A finding not fixed on this branch is a test of the expected behaviour, marked as an expected failure (`test.fail()` in Playwright) with the finding's title, so the suite passes today and flags the day the fix lands. A product question waiting for the user asserts today's behaviour, with a note.
- If the suite does not exist yet, create it as "Flow tests" in `docs/agents/end-to-end.md` describes, and fill that section's placeholders.
- Never put a password in a test. Read logins at run time, and skip with a message naming the missing file when they are absent.

## Apply the fixes

- Fix clear problems in the branch's own code, each with a failing test first (a server or unit test where the rule lives, and the flow test), as `AGENTS.md` asks of every bug fix.
- Propose, do not build, anything that changes a product rule, a decision, or the architecture; put it to the user with the options.
- A problem outside the branch: report it, with the steps, for its own branch.
- Never remove a feature, a test, history, a permission check, or a warning to make a journey pass, and never make a change that "Acting on findings" in `docs/agents/review.md` forbids.

## Finish

- Run the whole flow suite from the clean copy, not only the new journeys: every flow test passes, apart from those marked as expected failures.
- Report in the branch's review record (`docs/agents/review.md`): the journeys and people tested; each finding with its severity (critical when data reaches someone who must not see it, is lost, or changes without history; high when a journey cannot be finished; medium for wrong or misleading information or real friction; low for polish); what was fixed, with the commit; what waits for a decision; what could not be tested and why; and the suite's result.
- Stop your server and app; keep or drop your database copies, and say which.
