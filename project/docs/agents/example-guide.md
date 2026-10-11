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

## Sections a frontend guide needs

Write a frontend guide once the design direction is chosen (`docs/design/README.md`), because its rules come from that choice.
The `ui-check` skill checks screens against it.

- **Current stack.** The framework, where styles live, and what must not be added to the frontend as a side effect.
- **Design direction.** The chosen look as a few rules a reviewer can check, such as what the accent colour is for and what is never used.
- **Design tokens.** A table of the shared colours, sizes, spacing, and fonts, each with its use, and the contrast each text colour passes. New screens use these instead of hardcoded values.
- **Layout and screen widths.** The widths where the layout changes, what changes at each, and the widths to check every visual change at.
- **Accessibility.** The project's rules for labels, keyboard use, focus, contrast, and status shown by more than colour.
- **Content.** What placeholder content may show, and that controls for unbuilt features are disabled and say so.
- **Verifying UI work.** How to open the app and take screenshots at each width, and what to report.
