# End-to-end checks

Read this when a change touches something a browser, a device, or an operating system decides, rather than something your code decides.

## Why this exists

A test that exercises the code differently from how a person uses it can pass while the thing is broken.
Two shapes of that are common enough to plan for.

**The response is right and the page is still wrong.**
A server can answer correctly and the browser then decline to do what the answer asks.
A content security policy that forbids the redirect a form ends on is the clearest example: every request and response is correct, the whole suite is green, and the button does nothing.
Nothing in a response-level test can see this, because the refusal happens after the last correct response.

**What you tested is not what is running.**
A package installed as a copy does not follow the source it was built from.
Pull a fix, watch the same failure, conclude the fix is wrong — when the fix was never loaded.
Before deciding a change did not work, confirm the thing you ran is the thing you changed.

## When to run one

Run an end-to-end check when the change touches:

- Markup, response headers, a content security policy, a redirect, a form, a cookie, or anything else a browser interprets.
- A screen, a control, a permission prompt, or a system integration on a device or desktop platform.
- The path from a person's action to a result, where the pieces are each tested and the join is not.

A change to logic that has no surface does not need one.
This is not a rule that every feature gets an end-to-end test; it is a rule about where a particular class of defect hides.

## Which tool

Two tools, chosen once, so every session drives a surface the same way.

- **Playwright** for anything a browser runs: a web page, a web application, an embedded web view, or a response a browser is expected to act on.
- **Xcode and Swift** for anything built for an Apple platform: Swift Testing or XCTest for logic, XCUITest for the interface.

Never add a second browser driver or interface automation tool beside them, and never substitute a hand-rolled script that parses HTML for a real browser.
A project with both a web surface and an Apple app uses both tools, each for its own surface.

## In a browser

Playwright drives the browser.

TODO(template): The command that installs Playwright and its browsers, e.g. `npm install -D @playwright/test && npx playwright install chromium`.
TODO(template): The command that runs the browser tests, e.g. `npx playwright test`.

- **Drive the real thing.** Load the page, fill the fields, press the button. Assert on what the browser then does, not on what the server sent.
- **Assert that it moved.** "The browser left for the right address carrying the right value" is the assertion that catches a blocked redirect. "The response was a 303" is not.
- **Use a realistic viewport.** A phone-sized screen catches a warning nobody can read without scrolling, which is not a defence.
- **Keep the tooling optional.** Browser dependencies are large and the ordinary check command is run constantly. Make them an extra, and have the tests skip themselves with a reason that names the command to install them.
- **Never let them be why the suite cannot run.** A skipped browser test is a cost worth stating; a suite that will not start is not.

## On Apple platforms

Xcode and Swift run the tests.

TODO(template): The scheme and destination, e.g. `xcodebuild test -scheme <Scheme> -destination 'platform=macOS'`.
TODO(template): Whether the project uses XCTest, Swift Testing, or both.

- **Unit tests** (`XCTest` or `Swift Testing`) cover logic and belong with the rest of the suite.
- **UI tests** (`XCUITest`) drive the app as a person does, and are where the class of defect above shows up: a control that is present but not hittable, a sheet that never dismisses, a permission prompt nobody answered.
- **Run on the destinations you ship.** A simulator is not the device, and a macOS build is not an iOS build. Name the destinations in the command so the same ones run every time.
- **Sandbox and entitlements are part of the behaviour.** A file read that works in a test target and fails in the shipped app is this same class of bug: the code is right and the environment decides otherwise.
- **Keep the slow ones separate.** UI tests are minutes, not seconds. Give them their own command so the fast loop stays fast, and say in the review guide when they are expected.

## Reporting

Say which checks ran, on what, and what could not be checked.
"The suite passes" is not the same claim as "a browser did this", and the difference is the whole point of the file.
