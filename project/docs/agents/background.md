# Why the working rules exist

Template commit: TODO(template): the full 40-character commit hash of the template this project adopted or last updated from (`ADOPT.md`, "Recording the template commit").

The rules are in `AGENTS.md`; this file keeps the reasons, so a rule can be changed knowingly rather than guessed at.
Reasons for the review steps are in `docs/agents/review.md`, for Graphify in `docs/agents/graphify.md`, and for the decision file format in `docs/decisions/README.md`.
Add the reason for any rule this project adds to `AGENTS.md`, in the matching section below or a new one.

## One instruction file, with guides loaded on demand

- `AGENTS.md` is the only root instruction file and `CLAUDE.md` is a symlink to it, because separate copies drift apart when each is edited on its own.
- It stays short and routes to guides in `docs/agents/`, because a long instruction file loses adherence as it grows.
- Guides are written only once their code exists, because a guide for code that does not exist describes guesses and is wrong the moment the code arrives.

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

## Tests

- Coding agents often write tests, but not reliably: they skip them on small changes, test only the case that works, and under pressure weaken a failing test instead of fixing the code. A written rule makes tests part of done rather than a habit.
- A test written to fail before a bug fix proves the fix works, and keeps the bug from coming back unnoticed.
- Permission tests matter most, because a missing check fails silently: nothing breaks, someone just sees data they should not.

## Deadlines

- A deadline is a reason to cut features, never a reason to skip tests, reviews, the risk check, or history, because those are the hardest things to add back later.
- No target date is recorded, so that no document can be read as permission to rush.

## Writing for the reader

- Documents for people written in the compressed style of instruction files are harder to read than they need to be.
- A rule split by folder does not work, because `AGENTS.md` and decisions are read by both people and agents.
- Plain words cost a machine nothing, and jargon costs a person a lot, so a file both read is written plainly.
- One sentence per line keeps search matches to whole sentences.
