---
name: implement
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Implement the work described by the user in the spec or tickets.

If the user passes a ticket reference, fetch it from the issue tracker and state its title before starting. If the reference is ambiguous, ask.

Call the Skill tool with "tdd" where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

The **outside judge** decides done, not your own read of the diff: the repo's `.claude/verify.sh` if it has one, otherwise the full suite plus typecheck, plus any acceptance check the spec or ticket names. Run it, show its output, and don't proceed while it is red.

Once it passes, call the Skill tool with "code-review" to review the work.

Commit your work to the current branch.
