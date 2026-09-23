# Architecture

How this project is meant to be built, and why each part was chosen.
This is a living plan: edit it as the design changes, and let git history record how it changed.
What the product is for is in `docs/PRODUCT.md`, and what exists today is in `docs/SYSTEMS.md`.
Why a feature was built the way it was is in `docs/decisions/`, written when the feature is built.

Status: TODO(template): "planned", "partly built", or "built", with the date of the last meaningful update.
Anything still undecided is under "Still open" at the end, and stays open until the user decides.

## The parts and where they run

TODO(template): A table of the parts (for example browser app, server, database, file storage, sign-in), what each is built with, where it is hosted, and where it runs. Write "Not chosen yet" for any cell the user has not decided.

## Rules between the parts

TODO(template): What each part may and may not do. For example: which part checks permissions, which part may hold secrets, and which part talks to the database. Write "None yet" if nothing is decided.

## Repository layout

TODO(template): The top-level folders and what each holds, or "Not decided yet".

## Build order

TODO(template): The order the foundation and the first features are built in, following Phase 1 in `docs/PRODUCT.md`.

## Choices and why

Each choice records what was picked, why, and what was rejected, so a later reader, person or agent, can tell what they would be undoing.
Change a choice by editing its entry and saying why; git history keeps the old version.
Write the date any vendor fact (a price, a limit, a plan) was checked, because those change.

Format for each entry:

```markdown
### <Area>: <what was chosen>

- **Why.** <The reasons, with the numbers that support them.>
- **Rejected: <option>.** <Why it lost.>
- **Cost of this choice.** <What it makes harder, if anything.>
```

TODO(template): One entry per choice already made, or "None yet".

## Still open

TODO(template): Every choice not made yet, each with what it depends on. Agents never decide these on their own.
