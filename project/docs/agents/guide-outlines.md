# Guide outlines

What each task guide should hold, for the areas most projects grow.
Write a guide only once the code it covers exists (`AGENTS.md`, "Task guides"), start it from the matching outline below, and route it from the task guides table.
A guide is path-scoped: `paths:` front matter names the files it covers, with a link in `.claude/rules/` and a pointer skill in `.agents/skills/` (`AGENTS.md`, "Claude Code and Codex").
The rules under each outline held in a real project. Keep the ones that fit this project, and say in the pull request which you dropped and why.

## Every guide

- **Current stack.** What the code uses today, and what must not be introduced as a side effect of another change.
- **Conventions.** Naming, structure, shared helpers or tokens to reuse instead of reinventing.
- **Pitfalls.** Mistakes that are easy to make in this area and how to avoid them.
- **Verifying.** Exactly how to check work in this area before calling it done, and what to report.

## Frontend guide

Write it once the design direction is chosen (`docs/design/README.md`), because its rules come from that choice.
The `ui-check` skill checks screens against it.

- **Current stack.** The framework, where styles live, and what must not be added to the frontend as a side effect.
- **Design direction.** The chosen look as a few rules a reviewer can check, such as what the accent colour is for and what is never used.
- **Design tokens.** A table of the shared colours, sizes, spacing, and fonts, each with its use, and the contrast each text colour passes. New screens use these instead of hardcoded values.
- **Layout and screen widths.** The widths where the layout changes, what changes at each, and the widths to check every visual change at.
- **Accessibility.** The project's rules for labels, keyboard use, focus, contrast, and status shown by more than colour.
- **Content.** What placeholder content may show, and that controls for unbuilt features are disabled and say so. The two screen rules in "Reading" in the `ui-check` skill always apply.
- **Verifying UI work.** How to open the app and take screenshots at each width, and what to report.

## Testing guide

- **Where tests live and how they run.** Each kind of test (unit, server, browser, flow), its folder, and its command.
- **Fixtures.** The shared fixtures and factories. Tests write records the way the app does, through its own functions, not by inserting raw rows.
- **Real dependencies.** When the database enforces a rule (row security, triggers, constraints), tests run against a real database, never a fake, and fail rather than skip when it is missing.
- **What proves a protection.** A protection is tested by showing its test fails when the protection is removed; say so in the pull request. Expect the specific error: a test that passes on any error can pass for the wrong reason.
- **Every role.** Test a feature as each role, with the permissions it starts with, and as someone given or refused the permission the feature needs. Loop over every role rather than letting one stand for the rest. What a role may not do or see is a test too. A change to a role's starting permissions needs these tests for every feature it moves.
- **Dates and time.** Tests take their dates from one clock, never the machine's, and a date a view must show lies inside the period that view shows (a "tomorrow" is next week on a Sunday). Give the suite a setting that runs it as on another day, and use it for any change that handles dates.
- **Flow tests.** Where they live and how to run them (`docs/agents/end-to-end.md`, "Flow tests").
- **Synthetic data.** Invented names and reserved addresses such as `example.org`.

## Security guide

- **Must stay true.** The project's few security invariants, each one line. Typical ones:
  - Tenant boundaries are checked twice where possible: every query filters by the caller's account, and the database blocks it if one forgets.
  - A record that does not exist, one the caller cannot reach, and one archived all give the same "not found".
  - Tokens are checked for algorithm, issuer, audience, and expiry. A key list or sign-in service that cannot be reached is a server error, never a sign-in failure.
  - Permission checks run on the server, in one place. Hiding a button is not a check.
  - No one changes their own permissions, and no one grants a permission they do not hold.
  - Answers and logs never hold a password, a token, a database address, or a query's values. Refusals use fixed wording.
  - Settings and secrets come from the environment and fail loudly when missing, never with a default address or key.
- **Before merging.** Every new service function has an attempt from another account in the boundary tests. Every new endpoint has a test for no sign-in, for another account, and for a role without the permission.

## Data guide

- **A new table.** What every table must have: the tenant column, history of changes, how deletes are refused or turned into archiving, and the database grants the app's account gets. Name the test that fails until all of it is there.
- **Migrations.**
  - Keep the models in step with the migrations, with a test that compares them.
  - A new column on a table that may hold rows needs a default or a backfill.
  - Never edit a migration that has run anywhere; add a new one.
  - A migration merges in its own pull request, before the code that uses it. That code merges only after the migration's run has succeeded on every database, read from its log, not assumed from "finished".
  - The newly deployed server may run before the migration does, so its code must still work on the database as it was.
  - A migration that refuses existing data lists in its pull request what must be fixed in each live database first, and merges only after a person confirms it is done.
- **Writing records.** Every change names exactly one actor. Records are archived, not deleted, where history matters. Quantities carry a unit, and money is an integer in the smallest unit plus a currency.
