# Product areas

What each part of the product does, who can use it, and the rules it must keep.
One file per area, each with a header designed to be searched.
The strategy, the phase order, and the compliance status are in `docs/PRODUCT.md`, and that file wins when the two disagree.
What exists today is in `docs/SYSTEMS.md`, and why a built feature works the way it does is in `docs/decisions/`.

## Finding an area

Never read every file in this folder.
Search the headers first, then open only the files that match.

| Question | Command |
|---|---|
| One-line summary of every area | `rg --sort path -g "!README.md" "^summary:" docs/product/` |
| Areas in a phase | `rg -l -g "!README.md" "^phase:.*1" docs/product/` |
| Areas about the code you are changing | `rg -l -g "!README.md" -e "^touches:.*src/billing/" -e "^touches:.*src/" docs/product/` |
| A keyword anywhere | `rg -n --max-columns 200 "keyword" docs/product/` |

## The areas

| File | Area | Phase |
|---|---|---|
| None yet | | |

## Writing an area file

Create `docs/product/<area>.md` and add a row to the table above.

```markdown
---
area: <file name without .md>
phase: <phase number from docs/PRODUCT.md, or "all">
status: planned
touches: <code folders the area is built in, comma separated, or none until it is built>
summary: "<what the area does, in one sentence>"
---

# <Area name>

## What it is for
## First version
## Later
## Who can do what
## Rules that must stay true
## Open questions
```

- The header follows "The header rule" in `AGENTS.md`, and adds `area`, `phase`, and `status`.
- When an area is built, `touches:` names its code folders, so an agent changing that code finds the area.
- `status:` is `planned`, `building`, or `built`. When an area is built, `docs/SYSTEMS.md` describes what exists and this file keeps the plan.
- "First version" is what ships when the area's phase is built. "Later" keeps the rest out of that phase so it does not grow. A connection to another feature that the user did not take is written there as "suggested, not agreed" (`AGENTS.md`, "While you work").
- "Who can do what" names each role from "Users and roles" in `docs/PRODUCT.md`. The flow check tests each one.
- A file stays under about 150 lines. Past that, split the area into two files and update the table above.
- The project principles in `AGENTS.md` and the rules in `docs/ARCHITECTURE.md` apply to every area. Each file repeats only what is specific to it.
