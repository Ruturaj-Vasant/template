# Graphify guide

Read this when building, refreshing, or querying the code graph.
The Graphify rules in `AGENTS.md` (code-only, advisory) always apply.

## When to use it

Use the graph for architecture, dependency, data-flow, duplicate-implementation, and change-impact questions.
For narrow edits to files you already know, read them directly.
Documentation and decisions are never extracted into the graph; find them by header search.

## Building and refreshing

- A graph is worth building once the repository has enough application code that reading files to answer structural questions gets expensive.
- Build it once with `graphify extract . --code-only`.
- After code changes, refresh it with `graphify update .`, which uses local AST extraction only.
- Graphify flags change between versions. If a command fails, check `graphify --help` for the installed version rather than guessing.

## Querying

When `graphify-out/graph.json` exists:

- `graphify query "<question>"` for broad questions. Results are noisy; confirm them against source.
- `graphify explain "<symbol>"` and `graphify path "<A>" "<B>"` for known symbols.
- `graphify affected "<symbol>"` before changing shared code, to see what depends on it.

## Why code-only

- Code-only extraction is free, deterministic, and sends nothing to a model, which matters when private data may sit near the code.
- Semantic extraction of documents costs tokens on every refresh, produces inferred links that are guesses, goes stale with every change, and flattens the reasoning in plans and decisions.
- Code and decisions are linked without the graph: code comments cite decision IDs, and decisions list the paths they affect in `touches:`.
- `graphify claude install` is not used because it rewrites `CLAUDE.md`, and through the symlink `AGENTS.md`, and adds a hook.
