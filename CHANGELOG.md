# Changelog

What changed in the template, newest first.
Each entry says what an adopted project must do to take the change.
Projects apply the entries added after their recorded template commit (see `ADOPT.md`, Path 3).

## Every change to behavior ships with tests

- `project/AGENTS.md` "Definition of done" gains a testing rule: a feature ships with unit tests, permission or boundary tests, and failure-case tests in the same pull request; a bug fix starts with a failing test that reproduces it; tests use synthetic data; no test is deleted, skipped, or weakened to pass; browser- or device-decided changes also get an end-to-end check.
- `project/docs/agents/review.md` gains a matching scope check, and `project/docs/agents/background.md` gains "Tests" with the reasons.

**Adopted projects must:** add the rule above "Markdown files are notes" in `AGENTS.md`, the scope check line in `docs/agents/review.md`, and the "Tests" section in `docs/agents/background.md`. Name the project's own permission boundaries in the rule if it has them.

## Commit every change; Playwright for browser checks

- `project/AGENTS.md` and the template's own `AGENTS.md` gain a rule under "While you work" and "Rules": commit every change as soon as it is made, and push the branch whenever there is a remote, even when the work is unfinished or may never be merged. `project/docs/agents/background.md` gains "Committing" with the reason.
- `project/docs/agents/end-to-end.md` recommends Playwright as the default browser tool, says when to use its TypeScript or Python runner and what it cannot check, and uses Playwright commands as the placeholder examples.

**Adopted projects must:** add the commit rule to `AGENTS.md` and its reason to `docs/agents/background.md`. If the project has browser checks but no tool yet, consider Playwright, and record the choice in `docs/ARCHITECTURE.md`. A project that already uses another browser tool keeps it.

## Decisions are for features only; product and architecture plans; risk check

- Decisions now record only how and why a feature was built, written in the pull request that builds it. `project/AGENTS.md` gains "Where things are written" and "What belongs in a decision", and `project/docs/decisions/README.md` gains a "Not built" section in the decision format, a prompt under each heading, a "Why this format" section, and the rule that numbers are never reused.
- New `project/docs/PRODUCT.md` (what the project is meant to become, for whom, in what order, and a compliance table) and `project/docs/ARCHITECTURE.md` (the parts, where each runs, a "Choices and why" section with the reasons and rejected options for each choice, and "Still open"). Both are living plans, edited freely. `project/docs/SYSTEMS.md` "Not yet decided" now points to "Still open".
- New `project/docs/agents/background.md`: the reasons behind the working rules, and the `Template commit:` line. `project/docs/agents/review.md` and `project/docs/agents/graphify.md` gain "Why" sections.
- The template commit is no longer recorded in an adoption decision, because setting up is not a feature. `ADOPT.md` records it in `docs/agents/background.md` ("Recording the template commit"), and Path 3 still finds commits recorded the old way.
- `project/AGENTS.md` gains a "Risk check" before features (data, regulations, security risks, approvals, violations; stop only on a violation or a missing approval) and a rule that deadlines never justify skipping tests, reviews, the risk check, or history. The review guide adds security review whenever a risk check was needed, and two scope checks: the change follows `docs/ARCHITECTURE.md`, and a built feature has its decision file. The pull request template gains a "Risk check" section.

**Adopted projects must:**

1. Copy `docs/PRODUCT.md`, `docs/ARCHITECTURE.md`, and `docs/agents/background.md`, and fill their placeholders from what the project already knows. Never invent facts: leave "Unknown" or list the item under "Still open".
2. Sort existing decisions. Product strategy moves to `docs/PRODUCT.md`, architecture choices to "Choices and why" in `docs/ARCHITECTURE.md`, and the reasons behind working rules to `docs/agents/background.md` or the matching guide. Keep only feature decisions. Delete the moved files, never reuse their numbers, and say in `docs/decisions/README.md` which numbers were retired and where their content went. Update every citation of a retired number.
3. Put the template commit on the `Template commit:` line in `docs/agents/background.md`.
4. In `AGENTS.md`, add "Where things are written" with "What belongs in a decision", "Risk check" and its line in "Before you start", the deadline rule, the new task guide rows, and the new wording of definition-of-done items 2 and 3.
5. Update `docs/decisions/README.md`, `docs/SYSTEMS.md`, `docs/agents/review.md`, `docs/agents/graphify.md`, and `.github/pull_request_template.md` to match.

A project that already had its own risk check or plan files keeps them, and merges in only what is missing.

## End-to-end checks

- A new guide, `project/docs/agents/end-to-end.md`, covers the defects a response-level test cannot see: a correct response that a browser then refuses to act on, and a build artifact that is not the source you changed. It says when to run a real browser, what to assert, and how to keep the tooling optional so the ordinary check command stays fast. It has a section for Apple platforms too, covering XCTest, Swift Testing and XCUITest, and which destinations to name.
- `project/AGENTS.md` routes it from the task guides table and names it in the verify step of "Definition of done".
- `project/docs/agents/review.md` adds it to the steps a branch's risk can call for, beside security and performance.
- `ADOPT.md` gains a setting-up step for filling its placeholders and deleting the section that does not apply.
- To take this change: copy `docs/agents/end-to-end.md` into your project, fill its two or four placeholders with your own commands, delete the section for a platform you do not ship, and add the two routing lines to `AGENTS.md` and the one bullet to `docs/agents/review.md`. A project with no user-facing surface can skip all of it.

## Write for whoever reads the file

- `project/AGENTS.md` gains two rules under "While you work": documents people read use plain, direct English, files that mainly instruct an agent stay terse and structured, and a file both read is written plainly. The second rule asks for the same plain language when speaking to the user.
- The template's own `AGENTS.md` gains the matching rule, since `README.md`, `ADOPT.md`, and `CHANGELOG.md` are written for people.
- To take this change: copy the two bullets into your project's `AGENTS.md`, under the existing "one sentence per line" rule. Nothing else changes, and existing documents are not rewritten for this alone; bring them into line when you next edit them for another reason.

## Markdown files are notes; review steps follow risk; performance step

- `AGENTS.md` states that Markdown files are notes: they are never verified or reviewed, even on a branch that also changes code.
- `docs/agents/review.md` replaces "Documentation-only changes" with "Markdown files" and a new "Choosing the review steps" section: a bug fix gets code review, a feature gets simplify and code review, and security review and performance are added when the changes touch the areas listed there.
- The review order is simplify, performance, code review, security review, so code review sees the edits of both steps before it.
- New `.agents/skills/performance/SKILL.md`: fixes measurable, behavior-preserving performance problems in the diff and proposes larger ones (caches, pagination, index changes) instead of applying them. Claude Code reaches it through a `.claude/skills/performance/SKILL.md` link that `ADOPT.md` creates.
- `docs/agents/review.md` gains a "Project performance checks" placeholder.
- The simplify and security-review skills leave Markdown files out of their diffs and no longer call themselves step 1 or step 3.
- The pull request template's review record gains a Performance row and drops step numbers.

**Adopted projects must:** add the "Markdown files are notes" paragraph and update definition-of-done item 1 and the "Before merging" paragraph in `AGENTS.md`; in `docs/agents/review.md`, replace "Documentation-only changes" and the opening of "Pre-merge review" with the new "Markdown files", "Choosing the review steps", and "Pre-merge review" sections, update the Codex bullet in "Getting the scope right", add "Project performance checks" (filled in, or "None yet"), and update "Review record"; copy `.agents/skills/performance/`, create its `.claude/skills/performance/SKILL.md` link, and update the simplify and security-review skills; and update the review record table in `.github/pull_request_template.md`.
If the project already names security-sensitive or performance-critical areas elsewhere, list them in the matching project checks section rather than repeating them.

## Documentation-only branches skip verification and the pre-merge review

- `docs/agents/review.md` gains a "Documentation-only changes" section: a branch that changes nothing but Markdown files and symlinks to them skips the verification command and all three reviews, and records "Skipped: documentation only".
- The final verification after the reviews runs only if the tree changed since the last passing run.
- `AGENTS.md` definition of done, the pull request template, `README.md`, and `ADOPT.md` point to that rule.

**Adopted projects must:** add the "Documentation-only changes" section to `docs/agents/review.md` and remove its old "Security review may be skipped only when..." sentence; update definition-of-done item 1 and the "Before merging" paragraph in `AGENTS.md`; and update the review-record comment in `.github/pull_request_template.md`.
If the project treats some non-Markdown files as documentation, or some Markdown files as code, say so in that section.

## Initial template

- `AGENTS.md` working agreement with `CLAUDE.md` as a symlink.
- Task guides in `docs/agents/`: review, handoff, Graphify, and an example path-scoped guide with its Codex pointer skill. Symlinks are created during adoption, not shipped.
- `docs/SYSTEMS.md` for current state and `docs/decisions/` for one-file-per-decision records with searchable headers and header checks.
- Pre-merge review (simplify, code review, security review) for Claude Code and Codex, with Codex skills in `.agents/skills/` and a review record in `.github/pull_request_template.md`.

**Adopted projects must:** nothing; this is the starting point.
