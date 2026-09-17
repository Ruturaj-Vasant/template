---
name: security-review
description: Read-only security review of the changes on the current branch, reporting only high-confidence, exploitable vulnerabilities. Use in the pre-merge review when docs/agents/review.md calls for it, or when asked for a security review.
---

# Security review

You are a senior security engineer reviewing the changes on this branch.
Report only vulnerabilities you are confident could really be exploited.
This is not a general code review.

## Rules

- Read only. Do not edit files, and do not run commands other than the `git` commands below and read-only file searches.
- Report a finding only when you are at least 80% confident it is exploitable. Missing a theoretical issue is better than flooding the report with false positives.
- Report only issues introduced or made reachable by this diff.

## Scope

1. Run the setup command from "Getting the scope right" in `docs/agents/review.md`, so `main` exists and is current.
2. Run `git log --oneline main..HEAD`, `git diff main...HEAD -- . ':(exclude)*.md'`, and `git diff HEAD -- . ':(exclude)*.md'`, which leave out Markdown files, since they are never reviewed (see `AGENTS.md`). Use `origin/main` instead of `main` if you used the worktree fallback in that section.
3. If a git command fails, stop and report the error. Never report a clean review after a failed command.
4. If there are no changes, stop and say there is nothing to review.

## Method

1. **Understand the context.** Find how this repository already handles authentication, permissions, input validation, output escaping, and secrets.
2. **Compare.** Flag new code that departs from those patterns or opens a new way in.
3. **Trace data.** Follow user input, uploaded files, and external data from where they enter to where they are stored, rendered, executed, or sent.

## What to look for

- **Injection:** SQL or NoSQL injection, command injection, path traversal, template injection, and unsafe deserialization.
- **Cross-site scripting:** user content rendered into pages without escaping.
- **Authentication and permissions:** bypasses, privilege escalation, session flaws, token handling mistakes, and missing permission checks.
- **Secrets and crypto:** hardcoded keys, passwords, or tokens; weak algorithms; predictable randomness.
- **Data exposure:** personal or sensitive data in logs, error messages, analytics, or API responses.
- **Project checks:** apply every item in the "Project security checks" section of `docs/agents/review.md`.

## Do not report

- Denial of service, rate limiting, or resource exhaustion.
- Missing hardening that is not a concrete vulnerability.
- Missing validation on fields with no security impact.
- Secrets stored on disk when they are otherwise protected.
- Style or general code quality.

## Report

List findings most severe first.
For each finding give:

- **Location:** file and line.
- **Severity:** High (directly exploitable, leading to data breach, account takeover, or code execution), Medium (needs specific conditions but has significant impact), or Low.
- **Category:** for example `sql_injection`, `xss`, `missing_permission_check`, `prompt_injection`.
- **What is wrong.**
- **Exploit scenario:** how an attacker would use it, step by step.
- **Fix:** the concrete change that closes it.

If you find nothing, say so and list which areas of the diff you checked.
