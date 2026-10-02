# Stage gates, finish, and handoff

Read before marking a session `confirmed` or writing its handoff.

## Stage gates

Each stage ends with a version-controlled artifact that the next stage reads:

```text
intent.md → spec.md → plan.md → implementation + tests/evals
  → review/release record → deployment/incident record → new intent.md
```

Filenames may differ when the project already has an accepted system of record, but the handoff and linkage stay explicit. Name the human owner for each gate: the agent drafts and flags concerns, and the owner corrects, accepts, rejects, or records an explicit exception. If no canonical home has been chosen, make that decision explicit before claiming the stage is complete. Commit accepted artifacts and their linkage when the project uses version control; otherwise keep the approved system's revision or audit reference.

## Completion checklist

Before marking the session `confirmed`, go through the ledger item by item. The session is complete only when:

- the frontier and the blocked list are empty, and no fog remains inside the destination's scope;
- no item is left `pending`, `inferred`, or `unknown` unless it is explicitly kept as a named risk with an owner;
- every conflict and dependency has an owner or a stop condition;
- the canonical artifact is updated, or its required update is clearly identified;
- the ledger holds the final baseline, evidence, limitations, and a handoff; and
- the user or named owner confirms the baseline.

## Handoff

The handoff names the next step and where it happens, for example "write `plan.md` in a new grilling session" or "implement `plan.md` in a separate session". In a project with `.project-map/`, the next step after the decisions close is the spec, then slicing into build tickets (the `project-map` pipeline). Retain the ledger for auditability, and clear `ACTIVE.md` only when the user archives the session or selects another one. Completing the questions never authorizes consequential work, publishing, deployment, or production writes.
