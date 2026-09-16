---
paths:
  - "TODO(template)/**"
---

# Example guide

TODO(template): Rename this file and its Codex pointer skill to a real guide, such as `frontend`, and fill in its sections; see "Setting up the copied files" in the template's `ADOPT.md`.

This is the shape of a path-scoped task guide.
Claude Code loads it automatically when it reads a file matching `paths:` above, through a symlink in `.claude/rules/`.
Codex loads it when the task matches the description of the pointer skill in `.agents/skills/`.
Keep the rules here, never in the link or the pointer.

## Sections a guide usually needs

- **Current stack.** What the code uses today, and what must not be introduced as a side effect of another change.
- **Conventions.** Naming, structure, shared helpers or tokens to reuse instead of reinventing.
- **Pitfalls.** Mistakes that are easy to make in this area and how to avoid them.
- **Verifying.** Exactly how to check work in this area before calling it done, and what to report.
