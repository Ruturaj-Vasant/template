# Decisions

Why this project is built the way it is.
Each decision is one file, in the style of an Architecture Decision Record (ADR), with a header designed to be searched.
The header gives the fast, cheap answer; the body gives the full reasoning and is read only when it matters.

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
| Code that exists because of a decision | `rg "D-014" --glob '!docs/decisions/**'` |

To find the next decision number, list the files: `ls docs/decisions/D-*`.

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

## Decision

## Alternatives rejected

## Consequences

## Verification
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
