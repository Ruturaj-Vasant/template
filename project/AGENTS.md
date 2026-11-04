# Working agreement

Standing instructions for every coding agent in this repository (Claude Code, Codex, or otherwise).
`CLAUDE.md` is a symlink to this file: edit `AGENTS.md`, and never replace the symlink with a copy or add per-directory copies of these rules.
Do not remove or weaken instructions in `AGENTS.md` or `docs/agents/` without the user's explicit permission, including when cleaning up changes made by another agent.

## The project

TODO(template): Two to four sentences. What the project is, who it is for, what exists in the repository today, and how it is deployed. Say plainly what has not been chosen yet.

What the project is meant to become, for whom, and in what order is in `docs/PRODUCT.md`.
How it is meant to be built, and why each part was chosen, is in `docs/ARCHITECTURE.md`.

## Where things are written

| File | Holds | Changes |
|---|---|---|
| `docs/PRODUCT.md` | Product strategy: what we build, for whom, in what order, compliance status, with the reasons | Edited freely |
| `docs/product/` | Product areas: what each part does, who can use it, and the rules it keeps, one file per area | Edited freely |
| `docs/ARCHITECTURE.md` | Architecture plan and choices: the parts, where each runs, vendors, languages, frameworks, with the reasons and what was rejected | Edited freely |
| `docs/SYSTEMS.md` | What exists right now | Edited as things are built |
| `docs/decisions/` | Feature decisions: why a feature was built the way it was | Never edited once accepted |
| `docs/design/` | The chosen design direction and the mockups it came from | Edited when the design changes |
| `AGENTS.md`, `docs/agents/` | How we work: rules for agents, and the reasons behind them | Edited with the user's permission |

TODO(template): Files in `docs/` written only for people, such as research or a brief for a customer, which agents do not read unless the user asks, as one line ("`docs/<file>.md` is written for people, not agents. Do not read it unless the user asks."), or delete this line.

### What belongs in a decision

A decision records how and why a feature was built, so that anyone changing it later, a person or an agent, can answer "why is it like this?" without asking.

- Write one in the pull request that builds a feature or changes how it works.
- It says what was built, what was deliberately not built, the options considered and why each lost, the files it covers, and anything a later change must keep true.
- Test: if you cannot name a rejected option and the reason, it is not a decision.
- Never in a decision: product strategy (goes in `docs/PRODUCT.md`), choices of vendor, language, framework, database, or overall design (go in `docs/ARCHITECTURE.md` under "Choices and why"), descriptions of what exists (`docs/SYSTEMS.md`), or working rules (`AGENTS.md` and `docs/agents/`).
- When a feature changes a choice in `docs/ARCHITECTURE.md` or `docs/PRODUCT.md`, update that file too, with the reason.

The file format and searches are in `docs/decisions/README.md`.

## Finding context

Read the smallest piece that answers the question, and find it through an index before opening anything.

1. **Which file.** The table in "Where things are written" says what each file holds.
2. **Which file among many.** Every file in `docs/` that agents read starts with a header (the rule is below), except folder READMEs, which index their folder, and the task guides, which the "Task guides" table indexes. One search lists them all, one line each:

   ```bash
   rg --sort path -g "!README.md" "^(summary|decision):" docs/
   ```

   To find everything about the code you are changing, search `touches:` for its path and its parent folders:

   ```bash
   rg -l -g "!README.md" -e "^touches:.*src/billing/" -e "^touches:.*src/" docs/
   ```

   More searches are in `docs/product/README.md` and in "Finding decisions" below.
3. **Which section.** In a file longer than about 100 lines, list its headings with their line numbers, then read only the section you need:

   ```bash
   rg -n "^#{1,3} " docs/ARCHITECTURE.md
   ```

### The header rule

- A header is a block at the very top of a file, between two `---` lines.
- Every header has `summary:` and `touches:`. Decisions use `decision:` in place of `summary:`. A folder's README may add keys of its own.
- `summary:` says what the file holds, in one sentence wrapped in double quotes, with no double quotes inside.
- `touches:` lists the repository paths the file is about, comma separated, with folders ending in `/`, or `none`.
- Every value stays on one line, so a search match returns the whole value.
- A header describes the file as it is now. Change it in the same commit as the content it describes.

The header checks are in `docs/decisions/README.md`, "Checking the headers".

### When a file becomes a folder

- A file in `docs/` that passes about 300 lines, or whose sections are usually read one at a time, becomes a folder with one file per topic, each with a header, as `docs/product/` is.
- `docs/SYSTEMS.md` splits sooner, at about 150 lines per section (its opening says how).

Keep the indexes true.
A new file gets a row in the table and a header, and every heading names what its section holds, so the list of headings works as a table of contents.

## Before you start

- Run `git status`. Identify and preserve unrelated or uncommitted changes.
- Find the decisions that apply to the files you will change, and read `docs/SYSTEMS.md` and `docs/ARCHITECTURE.md` before significant work.
- Read every task guide below that matches the work, before editing.
- For a feature, open its product area in `docs/product/` (see "Finding context"), and keep the rules it lists.
- For a feature, do the risk check below before writing code.
- Before the first screen of a user interface is built, the user chooses a design direction, recorded in `docs/design/README.md`. Plan it with the `ux-design` skill, and never choose it for the user.
- For a feature with new screens or a change to navigation, plan its screens with the `ux-design` skill and get the user's approval before building them.

## Risk check

Before writing code for a feature, or changing how data is stored, shared, or sent anywhere, report a risk check to the user.
Skip it for changes that only touch documentation.

- **Data.** What the feature reads, stores, or sends, and how sensitive it is: for example confidential, personal, financial, or regulated.
- **Regulations.** Which rows of the compliance table in `docs/PRODUCT.md` may apply, including rows still marked unknown.
- **Security risks.** Account or tenant boundaries, role permissions (including what any AI feature can read), third-party services, uploaded files, and prompt injection.
- **Approvals.** Who must agree before it ships: the user, a customer, an outside review, or a vendor agreement.
- **Violations.** Anything in the request that would break a project principle, a current decision, the architecture plan, or a likely regulation, with a safer alternative.

If the check finds a violation or an approval not yet obtained, stop and ask the user before building.
Put the final risk check in the pull request description.

## Finding decisions

Feature decisions are one file each in `docs/decisions/`.
Never read the whole folder: search the headers, then open only the files that match.

```bash
rg --sort path "^decision:" docs/decisions/D-*          # one-line index of every decision
rg -l "^touches:.*<path or folder>" docs/decisions/D-*  # decisions about what you are changing; also search its parent folders
```

A decision named in another decision's `supersedes:` is no longer current.
`docs/decisions/README.md` has the other searches, the file format, and the header checks.

## Task guides

| When you are... | Read |
|---|---|
| TODO(template): The work a path-scoped guide covers, e.g. "Editing pages, styles, or UI" | TODO(template): The guide path, e.g. `docs/agents/frontend.md` (Claude Code loads it automatically; do not read it twice) |
| Choosing a design direction, planning a feature's screens, or auditing the UI | `.agents/skills/ux-design/SKILL.md` (`/ux-design` in Claude Code, `$ux-design` in Codex) |
| Changing anything a browser, a device, or an operating system decides | `docs/agents/end-to-end.md` |
| Finishing a feature, before merging into `main`, or asked to review code | `docs/agents/review.md` |
| Testing a feature's journeys as each kind of user, or adding flow tests | `.agents/skills/flowcheck/SKILL.md` (`/flowcheck` in Claude Code, `$flowcheck` in Codex) |
| Building or querying the code graph | `docs/agents/graphify.md` |
| Writing a prompt for another agent session | `docs/agents/handoff.md` |
| Starting to build, or choosing where new code goes | `docs/ARCHITECTURE.md` |
| Recording a feature decision | `docs/decisions/README.md` |
| Asking why a working rule in this file exists | `docs/agents/background.md` |

Other guides are written only once the code they cover exists, starting from the outlines in `docs/agents/guide-outlines.md`.
Until then, follow the project principles below and point out the missing guide in your report.

## Claude Code and Codex

Both tools read this file and the same guides in `docs/agents/`.
A path-scoped guide lives in `docs/agents/` and needs a symlink in `.claude/rules/` (Claude Code) and a pointer skill in `.agents/skills/` (Codex). Edit only the guide.

## Graphify

- Graphify is code-only. Never run semantic (LLM-backed) extraction, community labeling, a full rebuild, or `graphify claude install` unless the user explicitly asks.
- The graph is for navigation, not authority. Current source and passing tests win when it is stale or disagrees.
- The graph lives in `graphify-out/` on each machine and is never committed; build it if it is missing. `docs/agents/graphify.md` has the build and query commands.

## While you work

- Non-trivial changes go on a new branch, never directly on `main`. Changes reach `main` only through a pull request.
- Commit every change as soon as it is made, and push the branch whenever the repository has a remote, even when the work is unfinished or may never be merged. A commit saves progress; merging is a separate choice. Never end a session with work only in the working tree.
- Long work, such as several branches or full test runs, goes in its own git worktree (`git worktree add <folder> <branch>`), never in the main folder, which the user may switch to another branch mid-run. Before moving or resetting a branch, check with `git branch --contains` that no one else's commits would be lost.
- Keep the change inside the requested scope. Do not redesign unrelated areas.
- When planning a feature with the user, say how it could connect to features that exist or are planned, and what each connection would gain for the people using it. A suggestion the user does not take is written under its area's "Later" as "suggested, not agreed". Build a connection between features only when the user has said yes to it, never on your own.
- Deadlines never justify skipping tests, reviews, the risk check, history, or permission checks. If the work does not fit the time, say so and let the user cut features instead. No date is recorded as a target anywhere in the repository.
- Never credit an AI tool as an author or contributor: no `Co-Authored-By` trailers, "Generated with" lines, or similar attribution for Claude, Codex, or any other agent in commits, pull requests, code, or documentation.
- Never commit secrets, credentials, tokens, `.env` files, real user data, or other private data. Fixtures and examples use synthetic data only.
- Prefer a visible `unknown`, `incomplete`, or `unavailable` over a guessed value. Never silently swallow a failure.
- When code exists because of a decision, cite the ID in a comment next to it (`// D-042: search results are cached per user, not per query`).
- Match the surrounding code's idiom, naming, and comment density.
- Write prose one sentence per line, so a search match returns a complete sentence. State each rule in one place and link to it from elsewhere.
- Write for whoever reads the file. A document people read uses plain, direct English: short sentences, ordinary words, and any term explained the first time it appears. A file that mainly instructs an agent is terse and structured. A file both read is written plainly, because plain words cost a machine nothing and jargon costs a person a lot.
- Speak to the user the same way: simple language, and explain a term rather than assuming it.
- `docs/SYSTEMS.md` records what exists, `docs/PRODUCT.md` and `docs/ARCHITECTURE.md` record what is planned, and `docs/decisions/` records why a feature is the way it is; none repeats another (see "Where things are written").

## Project principles

These apply everywhere, including before their detailed guides exist.

TODO(template): Three to six rules that come from the project's domain and must never be broken, each as a bold one-line rule followed by one or two sentences. Examples of the kind of rule meant: "Money is stored as integer minor units with a currency code", "Records are never silently overwritten", "AI output is a draft until a named person approves it". Write only rules the user confirms; leave this as "None yet" rather than inventing them.

## Definition of done

TODO(template): The single command that must pass (build, lint, and tests), e.g. `npm run check`. If there is no toolchain yet, say so, and say to verify as the matching task guide describes and report exactly what was checked.

Once the flow suite exists, it runs too, before every merge (`docs/agents/end-to-end.md`, "Flow tests").

For every non-trivial change:

1. When a file other than Markdown changed, verify and report the results, including anything you could not check.
   If the change touches a surface a browser or a device decides, `docs/agents/end-to-end.md` says what that means and what to run.
2. Update `docs/SYSTEMS.md` when what exists changes, and `docs/PRODUCT.md` or `docs/ARCHITECTURE.md` when the plan changes.
3. When a feature was built or changed, write its decision as described in "What belongs in a decision", and run the header checks in `docs/decisions/README.md` after changing a header in `docs/`, or moving or renaming files.
4. If `graphify-out/graph.json` exists and code changed, run `graphify update .`.
5. Confirm `git status` shows only intended changes.
6. Report anything uncertain, untested, or intentionally out of scope.

**Every change to behavior ships with tests.**

- A feature ships with automated tests in the same pull request: unit tests for its logic, a test for each permission or account boundary the project has: another user's or tenant's data, and a role without access, and a test for each way it can fail (bad input, missing data, an outside service that is down).
- A bug fix starts with a test that reproduces the bug and fails; the fix then makes it pass.
- Tests use synthetic data only.
- Never delete, skip, or weaken a test to make it pass. If a test itself is wrong, fix it and say why in the pull request.
- Anything a browser or a device decides also gets an end-to-end check (`docs/agents/end-to-end.md`).
- A feature a person uses also gets its journeys as flow tests, one for each kind of user it serves and each other person its data reaches (`.agents/skills/flowcheck/SKILL.md`). The suite only grows.
- While no test tooling exists yet, list in the report the tests the change will need once it does.

**Markdown files are notes.**
Files ending in `.md` (including `AGENTS.md`, guides, decisions, and skills) are never verified or reviewed, even on a branch that also changes code: verification and every review step cover only the other files.
The header checks still run.

**Before merging into `main`:** run the review steps that `docs/agents/review.md` chooses for the branch's risk, in its order (simplify, performance, UI check, flow check, code review, security review), and put the review record in the pull request description.
A branch that changes only Markdown files records "Skipped: Markdown only" instead.
