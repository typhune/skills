---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, giving your recommended answer for each. Then wait for the user's answers before the next round.

When the `AskUserQuestion` tool is available, ask the round through it. Answering by picking an option is faster than reading and typing a reply.

- One tool question per frontier question. The tool takes at most 4 questions per call, so put a larger frontier into consecutive calls in the same round.
- Write each question and option in plain language: describe what the user would see or what happens, not code identifiers, flags, or names they haven't been shown.
- Give 2–4 options, each self-contained and answerable without scrolling back. Put your recommended answer first and append " (Recommended)" to its label. The user can always pick "Other" to type a free-form answer, so don't add one.
- Put the reasoning in each option's `description`. Keep `header` to 12 characters or fewer.
- When a question is open-ended and has no natural options, offer your best 2–3 candidate answers anyway; "Other" covers the rest.
- Text you write in the same message as the tool call may reach the user only as a one-line paraphrase. Put every fact a question rests on (what a sub-agent found, a price, a constraint) inside that question's text or option `description`s; use an option's `preview` for anything tabular or long.
- The closing shared-understanding summary is the one thing that can't fit in a question. Send it as a message on its own that ends your turn, and ask for confirmation in the next turn.

Without the tool, format a round as text like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
