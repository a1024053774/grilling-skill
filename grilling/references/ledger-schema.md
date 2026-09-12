# Grilling ledger schema

Use one Markdown file per session. YAML frontmatter makes the active state easy to scan; the sections below keep the human-readable record useful.

```yaml
session_id: grilling-<topic>-<yyyymmdd>
status: active
topic: <short topic>
created_at: <timestamp>
updated_at: <timestamp>
last_round: 0
```

Recommended sections:

```markdown
## Goal

## Non-goals

## Confirmed requirements and constraints

## Decision tree

## Decisions

<!-- D1 | state: confirmed | source: round 1 | rationale: ... -->

## Assumptions and facts

<!-- A1 | state: inferred | source: round 1 | evidence: ... -->

## Open questions

<!-- Q1 | state: pending | prerequisites: ... | recommended answer: ... -->

## Current frontier

## Risks and conflicts

## Final baseline

## Changelog

<!-- Round 1: ... -->
```

`ACTIVE.md` should contain only a pointer, for example:

```markdown
active_session: .grilling/payment-refactor.md
session_id: grilling-payment-refactor-20260912
```

When repairing a ledger, do not erase uncertain history. Add a recovery note, preserve the original content, and mark reconstructed entries `inferred` until the user confirms them.
