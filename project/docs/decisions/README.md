# Decisions

Why each feature of this project was built the way it is.
Each decision is one file, in the style of an Architecture Decision Record (ADR), with a header designed to be searched.
The header gives the fast, cheap answer; the body gives the full reasoning and is read only when it matters.

What belongs in a decision, and what goes elsewhere, is in `AGENTS.md` under "What belongs in a decision".
In short: features only, written in the pull request that builds them.
Product strategy is in `docs/PRODUCT.md`, architecture choices in `docs/ARCHITECTURE.md`, and working rules in `AGENTS.md` and `docs/agents/`.

## Finding decisions

Never read every file in this folder.
Search the headers first, then open only the files that match.

| Question | Command |
|---|---|
| One-line summary of every decision | `rg --sort path "^decision:" docs/decisions/D-*` |
| Decisions about a file or folder | `rg -l -e "^touches:.*apps/api/src/records" -e "^touches:.*apps/api" docs/decisions/D-*` |
| Decisions in an area | `rg -l "^areas:.*data-integrity" docs/decisions/D-*` |
| Which decisions have been replaced | `rg "^supersedes: *D-" docs/decisions/D-*` |
| A keyword anywhere | `rg -n --max-columns 200 "keyword" docs/decisions/D-*` |
| Code that exists because of a decision | `rg "D-042" --glob '!docs/decisions/**'` |

To find the next decision number, list the files (`ls docs/decisions/D-*`) and add one to the highest. Numbers are never reused, even when a decision is retired.

`touches:` often names a folder rather than a file, so search a path together with its parent folders, as in the table.

A decision named in another decision's `supersedes:` is no longer current, whatever its own `status:` says.

## Writing a decision

Create `docs/decisions/D-NNN-short-slug.md` with the next three-digit number.

```markdown
---
status: Accepted
date: YYYY-MM-DD
areas: <tags from the list below, comma separated>
touches: <repo-relative paths or folders, comma separated, or none>
supersedes: <D-NNN, comma separated, or none>
decision: "<the decision in one sentence>"
---

# D-NNN: <title>

## Context

What the feature is for, and what forced a choice.

## Decision

What was built and how, in enough detail that a reader can find it in the code.

## Not built

What was deliberately left out, and why.

## Alternatives rejected

Each option considered, and why it lost.

## Consequences

What a later change must keep true, and what this makes harder.

## Verification

How it was checked: tests, commands, and their results.
```

Header rules:

- Every key is present and each value stays on one line, so a search match returns the whole value.
- `decision:` is wrapped in double quotes and contains no double quotes of its own.
- `touches:` prefers folders over single files, uses paths relative to the repository root, and ends folders with `/`.
- `status:` is one of `Proposed`, `Accepted`, or `Rejected`.

Body rules:

- Write what the decision does not do as carefully as what it does.
- State the number that proves a claim rather than asserting the claim.
- Accepted decisions are never edited, except to fix a typo or a broken path. To change a decision, write a new one with `supersedes:`.
- `touches:` names the feature's folders, so an agent changing those files finds the decision.

## Why this format

- Agents find decisions by searching, and a search returns whole lines. A single log file of detailed entries grows to hundreds of KB, and a keyword search returns whole paragraphs.
- A one-sentence `decision:` makes `rg "^decision:"` a complete index at about 30 tokens per decision.
- `touches:` lets an agent find decisions by the file it is changing, instead of guessing the right words.
- `supersedes:` shows in one search whether a decision is still current, while accepted files stay unedited.
- `areas:` uses a fixed tag list, so tags do not drift into synonyms.
- Values stay on one line, because a YAML list or wrapped text breaks `rg "^key:.*value"`.
- Front matter rather than plain `Key: value` lines, because GitHub renders plain lines as one run-on paragraph.

## Areas

Use only these tags.
To add a tag, add it here in the same change that first uses it.

| Tag | Covers |
|---|---|
| `agents` | Agent instructions, documentation process, handoffs between sessions |
| `tooling` | Graphify, build, lint, test runners, developer scripts |
| `frontend` | Pages, styles, UI components, accessibility |
| `backend` | APIs, services, background jobs |
| `data-integrity` | Records, versioning, audit trail, units |
| `security` | Authentication, permissions, secrets, data exposure |
| `ai` | AI features, prompts, model use |
| `testing` | Test strategy and fixtures |
| `deployment` | Hosting, environments, release pipelines |
| `domain` | Concepts specific to this project's field. TODO(template): Replace with one or more domain tags, or delete the row |

## Checking the headers

Run these from the repository root after adding a decision or moving files.
Each prints nothing when everything is fine.

Files missing a header key:

```bash
for k in status date areas touches supersedes decision; do rg --files-without-match "^$k:" docs/decisions/D-*; done
```

`touches:` paths that no longer exist:

```bash
rg -N --no-filename "^touches:" docs/decisions/D-* | sed 's/^touches: *//' | tr ',' '\n' | sed 's/^ *//; s/ *$//' | rg -v '^none$' | sort -u | while read -r p; do [ -e "$p" ] || echo "missing: $p"; done
```

Tags not in the list above:

```bash
rg -N --no-filename "^areas:" docs/decisions/D-* | sed 's/^areas: *//' | tr ',' '\n' | sed 's/^ *//; s/ *$//' | sort -u | while read -r t; do rg -q "^\| \`$t\` \|" docs/decisions/README.md || echo "unknown tag: $t"; done
```
