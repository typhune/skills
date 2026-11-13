---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

The tree's root is the **outcome**, not the plan as framed. Open with a round-zero frontier of two questions: what should happen once this plan succeeds, and what is the goal beyond the goal (offer a higher-altitude restatement as your recommended answer when one fits); and what **outside judge**, other than you or the user, will show it worked (a test, a metric, a production signal). If the outcome answer changes the plan, the rest of the tree hangs off the new one. When an answer sets a bar ("under 5s", "fewer failures"), offer a 10× version as one option and say what it would take.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, giving your recommended answer for each. Then wait for the user's answers before the next round.

Ask the round **one question at a time**, each standing on its own: the user sees only the question in front of them while answering it, so every question carries what it decides, the facts it rests on (what a sub-agent found, a number, a constraint), the candidate answers, your recommendation and why. Write in plain language: describe what the user would see or what happens, not code identifiers or names they haven't been shown.

When the `ask_round` tool (from the grill-queue mod) is available, pass the whole frontier to it in one call. The mod hands back one question at a time, already formatted: show each exactly as given and end your turn. After the last answer it gives you the whole round's answers together.

Without it, ask in prose: one question per message, ending your turn, in the order the frontier allows. Hold each answer until the round is complete before recomputing the frontier. Format each like so:

```
❓ **Round <r>, question <i> of <n>: <question title>**

<what this decides and the facts it rests on, as many sentences as it needs>

- a. <candidate answer>
- b. <candidate answer>

➡️ <your recommended answer>, because <why>
```

Word each question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
