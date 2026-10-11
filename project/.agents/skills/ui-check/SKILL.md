---
name: ui-check
description: Check the screens changed on the current branch by looking at them in a browser, at every screen width, in every state, and by keyboard, and fix clear, local problems. Use in the pre-merge review when docs/agents/review.md calls for it, or when asked to check a screen's design, layout, or accessibility.
---

# UI check

You are checking that the changed screens are complete and usable, by looking at them, not by reading the code.
Fix clear, local problems, propose larger ones, and report what you saw.

## Scope

1. Run the setup command from "Getting the scope right" in `docs/agents/review.md`, so `main` exists and is current.
2. Run `git diff --name-only main...HEAD -- . ':(exclude)*.md'` and `git diff --name-only HEAD -- . ':(exclude)*.md'` to list committed and uncommitted changes, without Markdown files, which are never reviewed (see `AGENTS.md`). Use `origin/main` instead of `main` if you used the worktree fallback in that section.
3. If a git command fails, stop and report the error. Never report "nothing to check" after a failed command.
4. List the screens those files change. If there are none, stop and say so.
5. Only change code inside that diff and the code it calls directly.

## Rules

- Look at the result. Code that looks right is not evidence. If you cannot run the app or open a browser, say so and mark every check "not checked"; never mark it passed.
- The frontend guide named in the task guides table of `AGENTS.md` is the standard: its design direction, tokens, screen widths, and accessibility rules. The chosen design is in `docs/design/README.md`. If the project has no frontend guide yet, say so in the report.
- If the screen was planned with `ux-design`, the approved plan is the standard for its steps, states, and layouts.
- Never change what a screen does, its wording, or its navigation to make it look better; propose that instead.
- Never remove a feature, a test, error handling, input validation, or accessibility to fix a layout.
- Hiding a control is not a permission check. If a screen relies on hiding alone, report it as a security concern.

## What to check

At each screen width the frontend guide names, or a phone (about 400px), a tablet (about 800px), and a desktop (about 1280px) if it names none:

1. **States.** Loading, empty, with data, no results, invalid input, failure, not allowed, and success each look intended and say what to do next. Long names, many rows, and missing values do not break the layout.
2. **Layout.** The page never scrolls sideways. Nothing is cut off, overlaps, or hides behind a fixed bar. The main action is reachable on a phone. Tables stay readable on a phone, or change to a form that keeps their key fields and actions. Text fields are at least 16px on a phone, so the phone does not zoom in when one is tapped.
3. **Keyboard.** The main task works with the keyboard alone. Focus is visible and moves in a sensible order. Dialogs keep focus inside, close with Escape, and return focus where it was.
4. **Names and signals.** Every field has a label, and its error is tied to it. Every icon-only button has a name. A status never relies on colour alone. Text passes contrast against its background.
5. **Consistency.** Colours, sizes, and spacing come from the project's tokens, not new hardcoded values. Existing components are reused, not copied.
6. **Honesty.** No made-up figures or filler panels. A success message appears only after the action succeeded. A value the app does not know shows as unknown, not as a guess. A control for something not built yet is disabled and says so.
7. **Motion.** Animation respects the reduced-motion setting and never hides content.
8. **Console.** No errors in the browser console or failed requests while using the screen.

Take a screenshot of each changed screen at each width, and look at it.

## Apply the fixes

- For each finding, note the screen, the width, what is wrong, and the change.
- Apply the findings that are clear, local, and keep behavior identical, such as a missing label, a broken layout at one width, or a hardcoded colour that has a token.
- Leave larger changes as proposals for the user: a different layout, a new component, a change to navigation or wording, or anything that needs a design decision.
- Never make a change that the "Acting on findings" section of `docs/agents/review.md` forbids.
- Follow `AGENTS.md` and any task guide that matches the files you touch.

## Finish

Run the verification described in `AGENTS.md`.
Report each check as `check | screen and width | how you checked | passed, failed, or not checked | note`, then what you fixed, what you proposed without applying, and what you could not check and why.
Never call a screen "fully accessible", "pixel perfect", or "works offline" unless a check you ran shows it.
