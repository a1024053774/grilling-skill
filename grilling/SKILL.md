---
name: grilling
description: Stress-test an Agent project from intent through design, build, test, deploy, and maintenance in structured question rounds, with persistent state and version-controlled handoff artifacts.
---

# Grilling

Use this skill when a user wants a plan, decision, Agent project, architecture, or operational proposal challenged rigorously. It is project-agnostic: do not assume a particular product, model provider, framework, repository layout, or business domain.

The aim is a shared, reviewable baseline. Question the decisions that could change the outcome, expose missing evidence and unsafe assumptions, and stop at a clear human confirmation gate.

## Choose the mode and stage

First identify the user's entry point and the earliest incomplete stage:

| Entry point | Start with | Primary artifact |
| --- | --- | --- |
| Idea, problem, incident, or user pain | Plan / intent | `intent.md` |
| Accepted intent or requirements | Design | `spec.md` |
| Accepted spec or request to implement | Build planning | `plan.md` |
| Existing implementation or changed Agent behavior | Test / acceptance | eval cases and evidence record |
| Release or production request | Deploy | review, approval, and release record |
| Live system, incident, or drift | Maintain | incident/control-band record, then a new `intent.md` |

If several stages are in scope, work them in order and make the stage boundary explicit. Do not ask deployment questions while intent is still unresolved, and do not treat a completed conversation as an implementation or release approval.

For a standalone decision with no Agent lifecycle, use the same decision-tree and ledger protocol without inventing lifecycle artifacts.

## Persistent state and source of truth

Conversation history is not the source of truth. Before the first substantive question, inspect the current workspace for `AGENTS.md`, `CLAUDE.md`, an existing project ledger, `intent.md`, `spec.md`, `plan.md`, issue/PR records, and other declared project artifacts.

Use the project's declared canonical source when one exists. A `project-to-act` ledger, an approved `intent.md`, or an existing requirements system must not be silently duplicated. The `.grilling/` files are session control state and question history; they are not a competing project source of truth.

When no session ledger exists, create:

```text
.grilling/ACTIVE.md
.grilling/<topic-slug>.md
```

`ACTIVE.md` points to the active ledger. Read both before every round, including after a long pause or context compaction. Read [references/ledger-schema.md](references/ledger-schema.md) when creating or repairing them.

Before each new round:

1. Read the active ledger in full.
2. Read the current canonical project artifact for the stage, if one exists.
3. Apply confirmed decisions and constraints as the baseline.
4. Recompute the decision-tree frontier; ask only questions whose prerequisites are settled.
5. Record the previous answers, state changes, evidence, conflicts, and next frontier in the ledger before asking new questions.

If the active pointer or ledger is missing, preserve what exists, mark the session `recovery-needed`, reconstruct only from clear evidence, and ask the user to confirm the recovered baseline. Never silently infer lost decisions.

## What to record

Give every meaningful item a stable ID and state:

- `confirmed`: explicitly accepted by the user or the named project owner;
- `inferred`: a model interpretation awaiting confirmation;
- `pending`: a decision not yet answered;
- `rejected`: explicitly ruled out;
- `superseded`: replaced by a later decision;
- `unknown`: a fact that must be checked rather than guessed;
- `blocked`: required evidence, access, authorization, or environment is unavailable.

Keep these separate: goal, users, non-goals, success measures, constraints, requirements, decisions, assumptions, evidence, risks, open questions, dependencies, and current frontier. Record source, stage, round, and owner where relevant. Never silently turn an inference into a requirement or overwrite a conflict; link the affected IDs and put resolution on the frontier.

The ledger records the conversation. At a stage gate, the accepted result must also be written to the project's canonical artifact, with a link or commit reference from the ledger. Name the human owner for each gate: the Agent drafts and flags concerns, while the owner corrects, accepts, rejects, or records an explicit exception. If no canonical home has been chosen, make that decision explicit before claiming the stage is complete. When the project uses version control, commit accepted artifacts and their linkage; otherwise use the approved system of record and retain its revision or audit reference.

## Agent-project decision tree

Load only the branches applicable to the project. Mark a branch `N/A` with a reason when the project has no such capability; do not leave generic placeholders.

### Plan / intent

Establish:

- the problem, affected users, and why it matters;
- the desired outcome and observable success measures;
- in-scope and out-of-scope behavior;
- affected systems, data, and owners;
- constraints, policies, budget, latency, privacy, and safety boundaries;
- open questions and the smallest evidence needed to resolve them.

The output is a human-readable, machine-actionable `intent.md` or an equivalent canonical record. The originator corrects misunderstandings before it is accepted.

### Design / spec

Stress-test:

- Agent role, capabilities, limits, and human handoff;
- model/provider/version, prompt and Skill dependencies, and configuration;
- tools, permissions, external side effects, idempotency, and approval points;
- retrieval, knowledge, memory, source/version provenance, and citation behavior;
- orchestration, state transitions, queues, retries, replay, cancellation, and recovery;
- user interaction surfaces, accessibility, errors, and visible state;
- data handling, threat boundaries, abuse cases, cost and latency budgets;
- evaluation strategy, independent oracles, failure modes, and flagged policy concerns.

The output is an accepted `spec.md` or equivalent design record. Requirements and design may be discussed together, but unresolved policy conflicts remain visible.

### Build / plan

Require a written plan before implementation:

- files, services, tools, and interfaces that change;
- ordered implementation steps and ownership;
- migrations, configuration, dependencies, and rollback boundaries;
- tests/evals that prove each important contract;
- risks, alternatives rejected, and conditions that require plan revision.

The output is an accepted `plan.md`. When implementation departs from it, record the change in the same canonical chain rather than letting the plan become stale.

### Test / acceptance

Ask how the Agent will be shown to work through its real entry point:

- representative, unseen, adversarial, negative, and ambiguity cases;
- independent oracles for facts, permissions, state, citations, and actions;
- model/prompt/Skill/tool/data version and runtime evidence;
- tool postconditions, external readback, UI/backend consistency, and recovery;
- known-bad implementations that must fail, such as fixed output, ignored input, always-success, or test-only branches.

Separate `PASS`, `FAIL`, `BLOCKED`, `INCOMPLETE`, `NOT_RUN`, and `N/A`. Tests, screenshots, traces, model self-reports, and green counts are evidence fragments, not the acceptance oracle by themselves.

### Deploy

Resolve:

- environment, identity, permissions, secrets, and data boundaries;
- human approval and regulated/critical-action gates;
- hooks, CI/CD, staged rollout, monitoring, rate limits, and cost controls;
- rollback trigger, rollback owner, and a rehearsed recovery path;
- release record linking the accepted artifacts, candidate, review, and evidence.

Do not equate a merged diff or passing test suite with authorization to deploy.

### Maintain

Define:

- live control bands for quality, safety, latency, cost, capacity, and error rates;
- alerts, ownership, triage, human takeover, and incident severity;
- drift detection for prompts, models, tools, data, policies, and user intent;
- backup, recovery, retention, and replay boundaries where applicable;
- how a breach becomes a new version-controlled incident record and, when needed, a new `intent.md`.

## Evidence and fact handling

Facts are the agent's responsibility. Use authorized filesystem, repository, runtime, or external read-only tools to check facts; ask the user to choose decisions, not to supply facts that can be looked up. Label facts `observed`, `measured`, `inferred`, or `unknown`.

For every important question, state what evidence would change the decision, the smallest check that can obtain it, and the stop condition. If the necessary environment, oracle, authorization, or external boundary is missing, record `BLOCKED` or `INCOMPLETE`; do not fill the gap with a mock-only claim, fallback, or confident prose.

## Question rounds

The frontier is every decision whose prerequisites are settled. Ask the whole frontier in one round, number each question, and include your recommended answer. A recommendation is a proposal, never a confirmation.

Use this format:

```text
❓ **Q1** - **<question title>**: <question body, including choices or decision criteria>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body>

➡️ <your recommended answer>
```

After the user answers, map each answer to stable IDs, record explicit rejections and new inferences, resolve or expose conflicts, update the ledger, and then recompute the frontier. Set the ledger to `awaiting-user` while waiting and back to `active` when continuing.

Every few rounds, or whenever the goal or a major constraint changes, provide a compact drift check from the ledger: goal, non-goals, confirmed constraints, unresolved items, canonical artifact, and newly inferred items. Ask for correction only where the baseline changed.

## Stage gates and finish

Each stage ends with a version-controlled artifact that the next stage reads:

```text
intent.md
  → spec.md
  → plan.md
  → implementation + tests/evals
  → review/release record
  → deployment/incident record
  → new intent.md when the loop restarts
```

The exact filenames may differ if the project already has an accepted system of record, but the handoff and linkage must remain explicit.

A stage gate is complete only after its named human owner accepts the artifact or records a rejection/exception.

A grilling session is complete only when:

- the current stage's frontier is empty;
- important inferences and unknowns are confirmed, rejected, or explicitly retained as risks;
- conflicts and dependencies have owners or stop conditions;
- the canonical artifact is updated or its required update is clearly identified;
- the ledger contains the final baseline, evidence, limitations, and next stage; and
- the user or named owner confirms the baseline.

After confirmation, mark the session `confirmed`. Retain the ledger for auditability and clear `ACTIVE.md` only when the user explicitly archives the session or selects another one. Do not execute consequential work, publish, deploy, or write to production merely because questioning is complete.

Read [references/ledger-schema.md](references/ledger-schema.md) for the session ledger format. For acceptance-specific evidence, use the project's acceptance process or the `agent-acceptance-testing` Skill rather than duplicating its full test protocol.
