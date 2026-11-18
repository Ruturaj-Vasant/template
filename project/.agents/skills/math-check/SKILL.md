---
name: math-check
description: Write mathematical tests, rules that must hold for every input (a total equals the sum of its parts, a round trip keeps every value, an empty value is never 0, a check fails when a number is wrong), and let a property-testing library try to break them. Use when a change touches how data is parsed, calculated, checked, converted, or stored; when a new calculation or validation is added; when a bug in a value or a sum is fixed; or when asked for property tests, invariants, or arithmetic checks.
---

# Math check

You are writing the tests that state what must be true for every input, not only for the examples the suite already has.
An example test says "this input gives this output".
A property test says "for every input like this, this is true", and a property-testing library makes up hundreds of inputs to try to prove it false.
When it finds one, it shrinks it to the smallest input that still fails and prints it.
The library is TODO(template): the property-testing library for each language, e.g. Hypothesis for Python, fast-check for JavaScript and TypeScript.
These tests run with the automated tests (`AGENTS.md`, "Definition of done"); they live TODO(template): where property tests live, e.g. `tests/test_properties.py`.

## When to write one

- A change to how data is parsed, calculated, validated, converted, or stored.
- A new calculation or a new check that accepts or rejects data.
- A bug in a value or a sum: the fix's failing test is often better as a property, so the whole class of input is covered, not only the one example.
- Before trusting a tolerance or a limit: write down why it is big enough, then test that reason.

## What to state

Pick the rule first, in one sentence, and make it the test's name, so a failure reads as a sentence.
The kinds that fit most projects:

1. **Invariants: things that never change, whatever the input.**
   An empty or missing value is never turned into 0; text with no digit is never read as a number; a field the input does not have is never invented; a value never leaves its owner's account.
2. **Round trips: going there and back keeps everything.**
   Parse then format gives the input back; save then load gives the same record; export then import gives the same rows.
3. **Arithmetic identities: the sums the data itself must satisfy.**
   A total equals the sum of its parts; a percent equals the part over the whole; a balance equals the opening balance plus what came in minus what went out.
   Take the identity from the code's own validation, so the test proves what the code relies on.
4. **Unchanged by order or form: the answer does not depend on how the input was written.**
   Reordering the parts, the rows, or the columns gives the same answer; `1,000`, `$1,000`, and `1000` read the same.
5. **Sensitivity: the check fails when it should.**
   Change one part by more than the tolerance and the check must report it.
   A check that never fails proves nothing.
6. **Bounds: a tolerance is big enough, and why.**
   Write the reasoning in a comment above the test, as a short derivation (rounding to `d` places is off by at most half a unit in the last place, and what that does to a ratio), then test exactly the range the derivation covers.

A property test searches for a counterexample; it is not a proof.
Where the inputs are few, list them all in a parametrized test instead: checking every case is a proof.
Where they are not, the derivation in the comment is the proof and the property test checks it.

## How to write one

- Build inputs from generators that look like real data, then put them through the real code path, end to end where it is cheap, not only one helper.
- Use synthetic data only (`AGENTS.md`, "Every change to behavior ships with tests").
- Keep it fast: set the number of examples and the time limit per example so the property tests take seconds, because they run on every test run.
  Before committing, run them once with ten or twenty times the examples to shake out rare failures.
- Never call the network or a paid service from a property test.
- Leave out an input only when the rule truly does not cover it, and say why in a comment; never to hide a failure.

## When a property fails

Read the shrunk example and decide which is wrong: the code or the rule.

- **The code is wrong** and the fix is in scope: fix it, with the property as the failing test first.
- **The code is wrong** and the fix is outside the task: keep the property for the inputs it holds on, narrowed with a commented exclusion, and add the shrunk example as its own test marked as an expected failure that fails the suite once it passes (`pytest.mark.xfail(strict=True)`, `test.fails` in Vitest), with the reason and the date found.
  Report it.
- **The rule was stated too strongly:** restate it as it truly holds, fix the derivation in the comment, and keep the counterexample as a test of today's behaviour, or as an expected failure if that behaviour is a flaw.
- Never weaken a property to make it pass without one of the above, and never delete one.

## Finish

- The property tests pass, and so does every command in "Definition of done" in `AGENTS.md`.
- Report each rule added, each failure found with its shrunk example, and what was fixed, marked as an expected failure, or restated.
