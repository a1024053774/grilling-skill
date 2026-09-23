# Grilling ledger schema

Use one Markdown file per session. It is a map and an index: one line per item, with a link wherever the detail lives in a canonical artifact. YAML frontmatter makes the state easy to scan.

```yaml
session_id: grilling-<topic>-<yyyymmdd>
status: active            # active | awaiting-user | recovery-needed | confirmed
topic: <short topic>
destination_artifact: <path or system of record, e.g. spec.md>
created_at: <timestamp>
updated_at: <timestamp>
last_round: 0
```

Sections:

```markdown
## Destination

<one or two lines: what reaching the end of this grilling looks like>

## Out of scope

- <work ruled beyond the destination> | reason: ...

## Confirmed constraints

- C1 | state: confirmed | source: round 1 | ...

## Decisions so far

- D1 | state: confirmed | type: compare | source: round 2 | <one-line gist> | rejected: <options and why> | detail: spec.md#section

## Facts

- F1 | state: observed | source: <path, command, or URL, date> | ...

## Frontier

- Q7 | type: decide | why now: ... | recommended: ...

## Blocked

- Q9 | type: decide | blocked by: Q7, F3

## Not yet specified

- <a patch of fog: the suspected question or area to revisit>

## Risks and conflicts

- R1 | state: open | owner: ... | affects: D2, Q9

## Handoff

<final baseline, limitations, and the next step: what happens next, where, and who owns it>

## Changelog

- Round 1: ...
```

Rules:

- A question moves from Frontier or Blocked to Decisions so far when answered; it is never listed in two places. A fog patch is deleted when it graduates into questions.
- Prototype and research outputs are linked, not pasted.
- The ledger never records implementation steps, test runs, or release progress. Those belong to the canonical project record after the handoff.
- Before setting `status: confirmed`, every item must be `confirmed`, `rejected`, or `superseded`, or listed under Risks and conflicts with an owner.

`ACTIVE.md` contains only a pointer, for example:

```markdown
active_session: .grilling/payment-refactor.md
session_id: grilling-payment-refactor-20260912
```

When repairing a ledger, do not erase uncertain history. Add a recovery note, preserve the original content, and mark reconstructed entries `inferred` until the user confirms them.
