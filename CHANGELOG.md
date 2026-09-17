# Changelog

What changed in the template, newest first.
Each entry says what an adopted project must do to take the change.
Projects apply the entries added after their recorded template commit (see `ADOPT.md`, Path 3).

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
