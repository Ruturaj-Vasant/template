# Handoff guide

Read this when asked to write a prompt for another agent session, local or remote.

## What the prompt must contain

A new session starts with only this repository and your prompt.
It cannot see this conversation.

- The goal, and what done looks like, including the verification to run.
- The files, decisions (by `D-NNN` ID), and guides that matter, by path.
- Facts derived in this session that the repository does not record: measurements, constraints the user stated, and approaches already tried with the reason they failed.
- What is out of scope.

## How to run this

End every handoff prompt with a short **How to run this** block.
The same prompt produces very different work depending on where it runs.
Give each item a one-line reason, not a bare label.

- **Model.** The most capable model when the task needs judgment: design trade-offs, data modeling, or deciding what a result means. A faster model when the task is well specified and mechanical, and the hard thinking is already in the prompt.
- **Reasoning effort.** The highest effort when one subtle thing must be right and a plausible wrong answer would ship unnoticed. High for ordinary implementation. Low for rote edits.
- **New session or this one.** New when the prompt is self-contained and the work is long or context heavy, since a session that has already compacted will compact again and lose precise figures. This one when the work depends on context the prompt cannot carry.
- **Starting branch.** Name it. Remote sessions clone from GitHub, so unpushed work is invisible to them.
- **Credentials.** Name any secret the new session needs that is deliberately stored nowhere, so the user knows to paste it in.
