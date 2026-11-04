# Why the working rules exist

Template commit: TODO(template): the full 40-character commit hash of the template this project adopted or last updated from (`ADOPT.md`, "Recording the template commit").

The rules are in `AGENTS.md`; this file keeps the reasons, so a rule can be changed knowingly rather than guessed at.
Reasons for the review steps are in `docs/agents/review.md`, for Graphify in `docs/agents/graphify.md`, and for the decision file format in `docs/decisions/README.md`.
Add the reason for any rule this project adds to `AGENTS.md`, in the matching section below or a new one.

## One instruction file, with guides loaded on demand

- `AGENTS.md` is the only root instruction file and `CLAUDE.md` is a symlink to it, because separate copies drift apart when each is edited on its own.
- It stays short and routes to guides in `docs/agents/`, because a long instruction file loses adherence as it grows.
- Guides are written only once their code exists, because a guide for code that does not exist describes guesses and is wrong the moment the code arrives.

## Finding context

- Without an index, an agent reads whole files and folders to find one fact, which costs time and context on every run.
- A one-sentence `summary:` in every header makes one search a table of contents for the whole of `docs/`, at a few dozen tokens per file.
- `touches:` lets an agent find every document about the code it is changing, instead of guessing the right words.
- Long files are split into folders because an agent that needs one section otherwise reads the whole file.

## Claude Code and Codex

- Claude Code loads `.claude/rules/` files automatically when it reads matching paths.
- Codex reads `AGENTS.md` once at startup and has no path-scoped instructions. Nested `AGENTS.md` files only load when Codex is launched inside that folder, and Codex's `.codex/rules/` controls which commands may run, not instructions.
- So each path-scoped guide lives once in `docs/agents/`, with a symlink in `.claude/rules/` for Claude Code and a pointer skill in `.agents/skills/` for Codex.

## Where things are written

- Plans change often, but a decision is never edited once accepted, so a plan kept in decisions becomes a chain of superseded files. Product strategy therefore lives in `docs/PRODUCT.md` and the architecture plan in `docs/ARCHITECTURE.md`, both edited freely with git history as their record.
- Each architecture choice keeps its reasons and rejected options next to it in `docs/ARCHITECTURE.md`, so the reason is found where the choice is.
- Decisions are kept for features, because the question they answer is "why does this feature work like this?", asked by whoever changes it later.
- Plans are not kept in `AGENTS.md`, because every agent loads that file on every run.

## Risk check

- The pre-merge security review runs after code is written, which is too late to discover that a feature needs an outside review or a vendor agreement that takes weeks.
- It finds missing approvals and regulatory scope, which a code review does not.
- The agent stops only on violations and missing approvals, rather than asking before every feature, so low-risk work is not slowed.
- The check is only as good as the compliance table in `docs/PRODUCT.md`, and it is not legal advice.

## Committing

- Work that exists only in a working tree is lost when a laptop fails, a disk is wiped, or a remote session's container is reclaimed.
- A commit on a pushed branch costs nothing and can be dropped later, so saving progress never waits for the work to be finished or for a decision to merge it.

## Worktrees

- A user who switches the main folder to another branch while an agent's test run is going changes the code under the run, and its results test a mix of two branches. A worktree of its own keeps an agent's long work apart.

## Connections between features

- An agent that links features on its own leaves surprises in the code: a change to one feature quietly changes another.
- An agent that never mentions connections leaves a product of parts that do not work together.
- Suggesting each connection, and building only those the user agrees to, keeps the parts working together without surprises. A suggestion not taken is written down, so it is not suggested again as new.

## Tests

- Coding agents often write tests, but not reliably: they skip them on small changes, test only the case that works, and under pressure weaken a failing test instead of fixing the code. A written rule makes tests part of done rather than a habit.
- A test written to fail before a bug fix proves the fix works, and keeps the bug from coming back unnoticed.
- Permission tests matter most, because a missing check fails silently: nothing breaks, someone just sees data they should not.
- Each journey the flow check tests is kept as a flow test, so the suite grows with every feature and each later change is checked against every earlier journey. A known problem stays as a test marked as an expected failure, so it is neither lost nor hidden. Why the flow check is its own review step is in `docs/agents/review.md`.

## User interface

- Coding agents build screens that pass their tests and are still hard to use: the state with data is built and the empty, failed, and not-allowed states are forgotten, a layout breaks on a phone, or a control cannot be reached by keyboard. These are found only by looking at the screen, so the review has a UI check.
- Without a chosen look, each agent session invents its own, and a product drifts into a mix of styles. Choosing a design direction before the first screen, and writing its rules as tokens in the frontend guide, gives every session the same standard.
- The user chooses the direction from two or three mockups, because taste is the user's call, and comparing options shows what they want faster than describing it.
- Screens are planned before they are built, because changing a plan costs minutes and changing built screens costs a pull request.
- Sentences that explain the product's own rules on screen, such as "no one is ranked", clutter the page and confuse the people using it; the rules belong in how the screen works and in the docs.
- A fact shown twice in one window makes people read more to learn nothing new, so each fact shows once.

## Deadlines

- A deadline is a reason to cut features, never a reason to skip tests, reviews, the risk check, history, or permission checks, because those are the hardest things to add back later.
- No target date is recorded, so that no document can be read as permission to rush.

## Writing for the reader

- Documents for people written in the compressed style of instruction files are harder to read than they need to be.
- A rule split by folder does not work, because `AGENTS.md` and decisions are read by both people and agents.
- Plain words cost a machine nothing, and jargon costs a person a lot, so a file both read is written plainly.
- One sentence per line keeps search matches to whole sentences.
