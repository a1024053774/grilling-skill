# Agent-project lifecycle branches

Read only the section for the current stage. Mark a branch `N/A` with a reason when the project has no such capability; do not leave generic placeholders. Each branch ends in the artifact named in the SKILL.md stage table.

## Plan / intent

Establish:

- the problem, affected users, and why it matters;
- the desired outcome and observable success measures;
- in-scope and out-of-scope behavior;
- affected systems, data, and owners;
- constraints, policies, budget, latency, privacy, and safety boundaries;
- open questions and the smallest evidence needed to resolve them;
- the risk tier of the work, which sets how much review and automation later stages get: low-risk work can be self-approved, higher tiers need a named human reviewer.

The output is a human-readable, machine-actionable `intent.md` or an equivalent canonical record. The originator corrects misunderstandings before it is accepted.

## Design / spec

Stress-test:

- Agent role, capabilities, limits, and human handoff;
- model/provider/version, prompt and Skill dependencies, and configuration;
- tools, permissions, external side effects, idempotency, and approval points;
- retrieval, knowledge, memory, source/version provenance, and citation behavior;
- orchestration, state transitions, queues, retries, replay, cancellation, and recovery;
- user interaction surfaces, accessibility, errors, and visible state;
- data handling, threat boundaries, abuse cases, cost and latency budgets;
- evaluation strategy, independent oracles, failure modes, and flagged policy concerns.

For each load-bearing design choice, put at least two credible alternatives on the frontier as a `compare` question before converging, and escalate to a `prototype` question when talking cannot separate them. The spec records the chosen option, the rejected ones, and why.

The output is an accepted `spec.md` or equivalent design record. Requirements and design may be discussed together, but unresolved policy conflicts remain visible.

## Build / plan

Require a written plan before implementation:

- files, services, tools, and interfaces that change;
- ordered implementation steps and ownership, starting with a thin end-to-end slice when the work spans layers;
- migrations, configuration, dependencies, and rollback boundaries;
- tests/evals that prove each important contract;
- risks, alternatives rejected, and conditions that require plan revision.

The output is an accepted `plan.md`. Implementation happens after the handoff, outside the grilling session. When implementation later departs from the plan, the change is recorded in the same canonical chain rather than letting the plan go stale.

## Test / acceptance

Ask how the Agent will be shown to work through its real entry point:

- representative, unseen, adversarial, negative, and ambiguity cases;
- independent oracles for facts, permissions, state, citations, and actions;
- model/prompt/Skill/tool/data version and runtime evidence;
- tool postconditions, external readback, UI/backend consistency, and recovery;
- known-bad implementations that must fail, such as fixed output, ignored input, always-success, or test-only branches.

Separate `PASS`, `FAIL`, `BLOCKED`, `INCOMPLETE`, `NOT_RUN`, and `N/A`. Tests, screenshots, traces, model self-reports, and green counts are evidence fragments, not the acceptance oracle by themselves. Grilling settles the acceptance design; running it belongs to the project's acceptance process or the `agent-acceptance-testing` Skill.

## Deploy

Resolve:

- environment, identity, permissions, secrets, and data boundaries;
- human approval and regulated/critical-action gates;
- hooks, CI/CD, staged rollout, monitoring, rate limits, and cost controls;
- rollback trigger, rollback owner, and a rehearsed recovery path;
- release record linking the accepted artifacts, candidate, review, and evidence.

Do not equate a merged diff or passing test suite with authorization to deploy.

## Maintain

Define:

- live control bands for quality, safety, latency, cost, capacity, and error rates;
- alerts, ownership, triage, human takeover, and incident severity;
- drift detection for prompts, models, tools, data, policies, and user intent;
- backup, recovery, retention, and replay boundaries where applicable;
- separate identities with least agency: an agent that diagnoses incidents may draft a fix but cannot deploy it, and the fix goes through the normal reviewed path;
- how a breach becomes a new version-controlled incident record and, when needed, a new `intent.md`, and how a repeated failure class becomes a rule in the project's instructions.
