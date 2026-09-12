---
name: grilling
description: Stress-test a plan, decision, or idea through structured rounds of questions, with a persistent decision ledger that survives long conversations and context compaction.
---

# Grilling

Use this skill when the user wants an idea, plan, decision, architecture, or proposal challenged rigorously. The goal is shared understanding, not argument for its own sake.

## Persistent state is mandatory

Conversation history is not the source of truth. Every session must have a ledger in the current workspace:

```text
.grilling/ACTIVE.md
.grilling/<topic-slug>.md
```

`ACTIVE.md` contains the relative path of the active session ledger. Keep one active session unless the user explicitly asks to work on more than one. Do not put the ledger in `AGENTS.md`; that file is repository policy, while the ledger is mutable session state.

Before every round, including after context compaction or a long pause:

1. Read `.grilling/ACTIVE.md` when it exists.
2. Read the referenced ledger in full.
3. Treat `confirmed` decisions and requirements in the ledger as the current baseline.
4. Recompute the decision-tree frontier from the ledger. Ask only questions whose prerequisites are settled.
5. Write the user's previous answers, state changes, and the next frontier to the ledger **before** asking the next round.

If no active ledger exists, create `.grilling/` and a new ledger before asking substantive questions. If the active path is missing or malformed, preserve the file, mark the session `recovery-needed`, reconstruct only from clearly available conversation evidence, and ask the user to confirm the recovered baseline before proceeding.

Read [references/ledger-schema.md](references/ledger-schema.md) when creating, repairing, or substantially updating a ledger.

## Ledger rules

Record each meaningful item with a stable ID and an explicit state:

- `confirmed`: the user explicitly accepted it;
- `inferred`: a model interpretation that still needs confirmation;
- `pending`: a decision or question not yet answered;
- `rejected`: the user explicitly ruled it out;
- `superseded`: replaced by a later decision;
- `unknown`: a fact that must be checked rather than guessed.

Keep goals, non-goals, requirements, constraints, decisions, assumptions, evidence, risks, open questions, and the current frontier separate. Record the source and round for important entries. Never silently turn an inference into a confirmed requirement, and never silently overwrite a conflicting decision: append the conflict, link the affected IDs, and put resolution on the frontier.

Facts are the agent's responsibility. Use the available filesystem, repository, or other authorized tools to check facts; ask the user to choose decisions, not to provide facts that can be looked up. Delegate a bounded fact lookup only when it materially unblocks the current frontier and delegation is available.

## Session lifecycle

Use these ledger statuses:

`active` → `awaiting-user` → `active` while questions remain; use `recovery-needed` for an integrity problem, `ready-for-confirmation` when the frontier is empty, `confirmed` after the user accepts the final baseline, and `archived` when the user is done.

At the end of each round, update `last_round`, the current frontier, and a short changelog entry. Before a response likely to be followed by context compaction, write a checkpoint containing the full baseline and unresolved items. On resumption, trust the checkpoint over memory and mention any recovered uncertainty.

When the frontier is empty, write a final baseline and ask the user to confirm it. Do not execute consequential work based on the grilling results until that confirmation is received. After confirmation, mark the ledger `confirmed`; retain it for auditability and clear `ACTIVE.md` only when the session is explicitly archived or another session is selected.

## Question rounds

Map the discussion as a **decision tree**: every decision branches into the decisions that hang off it. Work the tree in **rounds**. The frontier is every decision whose prerequisites are already settled. Ask the whole frontier in one round, number each question, and include a recommended answer. Do not ask a downstream question in the same round as an unresolved prerequisite.

Use this format:

```text
❓ **Q1** - **<question title>**: <question body, including choices or the decision criteria>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body>

➡️ <your recommended answer>
```

After the user answers, map each answer to the relevant stable IDs, record explicit rejections and new inferences, resolve conflicts, update the ledger, and then recompute the frontier. Do not ask the next round until those writes are complete.

Every few rounds, or whenever the user's answer changes the goal or a major constraint, include a compact drift check from the ledger: goal, non-goals, confirmed constraints, unresolved questions, and newly inferred items. Ask for correction only where the baseline has changed.

## Finish condition

The session is complete only when:

- the decision-tree frontier is empty;
- no important `inferred` or `unknown` item is being treated as settled;
- conflicts are resolved or explicitly accepted as risks;
- the ledger contains the final baseline, assumptions, evidence, and remaining risks; and
- the user confirms that baseline.

If the user asks to act before confirmation, explain which decisions remain unconfirmed and ask whether to proceed with the stated uncertainty; do not pretend that the grilling is complete.
