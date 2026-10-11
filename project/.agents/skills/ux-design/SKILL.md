---
name: ux-design
description: Plan how a user interface works before it is built. Use to choose a project's design direction before its first screen, to plan the screens and steps of a feature, to audit existing screens, or to propose changes to navigation. Produces plans and mockups for the user to approve; never changes application code.
---

# UX design

You are planning how people will use the product: what they see, in what order, and what happens when things go wrong.
The result is a plan the user approves, not code.
Building the approved plan follows `AGENTS.md` and the project's frontend guide.

## Rules

- Never change application code in this skill. Write only plans, mockups, and the files named below.
- Get the user's approval before anything is built from a plan, and before any change to navigation or to how an existing screen works.
- Keep three things apart in everything you report: **Observed** (seen in the code or the running app, with the file or screen), **Proposed** (a recommendation, with the reason and who it affects), and **Needs decision** (a choice, a trade-off, or an assumption only the user can settle).
- Never claim a test with real users, a design file, or a prototype exists unless you made it or saw it.
- Never invent features, figures, or content. Mockups use clearly made-up data, and show only what exists or is planned in `docs/PRODUCT.md`.
- Reuse the project's existing screens, components, and design tokens (the shared colours, sizes, and spacing in the frontend guide) before proposing new ones.
- Follow `AGENTS.md`, including the project principles and the risk check.

## Start from the project's own facts

Read only what the task needs:

1. `docs/PRODUCT.md`: the users, their roles, the tasks each does most often, and what is planned.
2. `docs/design/README.md`: the chosen design direction, if there is one.
3. The frontend guide named in the task guides table of `AGENTS.md`, if it exists: tokens, screen widths, and accessibility rules.
4. The screens the task touches, in the code and, when the app can be run, in a browser.

If a fact is missing, say so and list it under "Needs decision". Ask only when the gap blocks a safe plan.

## Choosing a design direction

`AGENTS.md` requires this before a project's first screen is built.

1. Ask the user who the product is for, how it should feel, and any product they admire or dislike.
2. Make two or three clearly different directions, each as an image or a single working HTML page in `docs/design/`, showing the same realistic screen with made-up content. Show a phone-sized and a desktop-sized layout for each.
3. For each, name it and describe it in a sentence: colours, type, density, and the feel it aims for.
4. The user chooses, or asks for another round. Never choose for them.
5. Record the choice in `docs/design/README.md`: the chosen direction, the date, the ones that lost and why, and the files.
6. Write or update the frontend guide from the choice: the design direction as a few rules, the design tokens, the screen widths where the layout changes, accessibility, content rules, and how to check UI work. The guide is the single source of these rules; `docs/design/README.md` only points to it.

## Planning a feature's screens

1. **The problem.** Who uses it, what starts it, what they are trying to get done, and what a mistake costs.
2. **Where it lives.** Where it sits in the navigation, how people reach it, and how they get back. Show each role only what its main tasks need, and keep rarely used options one step away rather than on every screen.
3. **The steps.** For each main task: where it starts, each action or choice, the result, and who may do each step.
4. **Every state.** For each screen, fill this table:

   | State | What the person sees | What they can do next |
   |---|---|---|
   | Loading | | |
   | Empty (nothing yet) | | |
   | With data, including very long names and many rows | | |
   | No results for a search or filter | | |
   | Invalid input | | |
   | Something failed, such as the network or the server | | |
   | Not allowed, for each role without access | | |
   | Success | | |

5. **Layouts.** A low-detail sketch of each screen, as ASCII or a single HTML page, at each screen width the frontend guide names (a phone, a tablet, and a desktop if it names none). Say what the main action is, what is secondary, and how tables or dense lists change on a phone.
6. **Components.** Which existing components and tokens it reuses, and anything new it needs, with the reason.
7. **Acceptance criteria.** For the top tasks, "Given, When, Then" statements that a test or a person can check.
8. **Alternatives.** For a change to navigation or to how an existing screen works, at least one alternative, with its trade-off and your recommendation.

Write the approved plan into `docs/PRODUCT.md`, under the feature it belongs to, or into the project's product area file if the project splits them.
Mockups go in `docs/design/`.
How and why the feature was finally built goes in its decision when it is built, as `AGENTS.md` says.

## Auditing existing screens

1. List the screens and group them: main tasks, supporting tasks, and settings or administration.
2. Walk through at least one frequent task, one task with several steps, one empty or failure path, and one role with limited access.
3. Look for: labels people would not understand, actions hard to find, unclear current location, missing states, missing feedback after an action, errors without a way to recover, colour as the only signal, keyboard traps, and layouts that break on a phone.
4. Look for too much at once: more choices on a screen than its main task needs, rows of tabs or buttons a person must read through, icons that do not help someone understand faster, options most people rarely use shown to everyone all the time, and steps a person could skip. Count the tabs, buttons, and steps on each screen, and say which ones its main task uses.
5. Tell a design problem apart from a bug, a missing feature, a product decision, and a permission problem on the server. Report the last kind to the user as a security concern, never as design.
6. Report each finding as `screen | role and task | what you observed | impact | severity | proposed fix | confidence`, most severe first.
   Severity: **critical** blocks an important task or causes a serious data or security mistake; **high** makes a frequent task fail or an essential flow unusable without a mouse or a screen; **medium** is real friction or an error-prone step; **low** is polish.
7. End with the five most important findings and a small first set of fixes to approve.

## Finish

Report what you planned or found, split into Observed, Proposed, and Needs decision, with the files you wrote.
A plan is ready to build only when the roles and their main task are clear, every state is described, the phone layout is defined, permissions and recovery from errors are covered, and the acceptance criteria can be checked.
If any of these is missing, say which, rather than calling the plan done.
