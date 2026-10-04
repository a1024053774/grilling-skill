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

> <the user's own words for it and for the goal behind it, quoted verbatim>

## Out of scope

- <work ruled beyond the destination> | reason: ...

## Confirmed constraints

- C1 | state: confirmed | source: round 1 | ...
- C2 | state: confirmed | priority | source: round 3, D4 vs D7 | when <A> and <B> collide, <A> wins

## Decisions so far

- D1 | state: confirmed | type: compare | origin: user | source: round 2 | <one-line gist> | rejected: <options and why> | detail: spec.md#section
- D2 | state: confirmed | type: decide | origin: recommended | source: round 3 | <one-line gist> | basis: C1 | assumes: <assumption the recommendation carried>

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
- `origin: recommended` marks a decision the user took from the agent's recommendation without adding a reason of their own. It counts as confirmed; the drift check lists it separately so the user can see which parts of the baseline are borrowed.
- The ledger never records implementation steps, test runs, or release progress. Those belong to the canonical project record after the handoff.
- Before setting `status: confirmed`, every item must be `confirmed`, `rejected`, or `superseded`, or listed under Risks and conflicts with an owner.

## Item states

Give every meaningful item a stable ID and one state:

- `confirmed`: explicitly accepted by the user or the named project owner;
- `inferred`: a model interpretation awaiting confirmation;
- `pending`: a decision not yet answered;
- `rejected`: explicitly ruled out;
- `superseded`: replaced by a later decision;
- `unknown`: a fact that must be checked rather than guessed;
- `blocked`: required evidence, access, authorization, or environment is unavailable.

## Pointer and repair

`ACTIVE.md` contains only a pointer, for example:

```markdown
active_session: .grilling/payment-refactor.md
session_id: grilling-payment-refactor-20260912
```

If the pointer or ledger is missing, preserve what exists, mark the session `recovery-needed`, reconstruct only from clear evidence, and ask the user to confirm the recovered baseline. When repairing a ledger, do not erase uncertain history. Add a recovery note, preserve the original content, and mark reconstructed entries `inferred` until the user confirms them.
