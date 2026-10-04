# Question round format

Read before posting the first round. Number each question across the round, keep the evidence detail in the ledger, and separate questions with a rule. When an option or term could be read two ways, add a one-line concrete example. Recommendations are where drift starts: each one looks right alone, and once accepted, its hidden assumptions become the premises of the next round. The `Basis` line under each recommendation makes them visible before the user accepts them.

Open the reply with any conflict the last answers exposed, before the new questions:

```text
⚠️ **Conflict** - D4 (<gist>) vs D7 (<gist>): <why both cannot hold>. Asked as Q9 below.
```

Then the round:

```text
❓ **Q1** - **<question title>** `<type>`: <the choice and the criteria or options>

Why now: <what this unblocks, or what breaks if it is assumed>

➡️ <your recommended answer>
Basis: <the destination, D/C IDs it follows from, or "assumption: ...">

---

❓ **Q2** - **<question title>** `<type>`: <the choice and the criteria or options>

Why now: <what this unblocks>

➡️ <your recommended answer>
Basis: <...>
```

## Drift check

Quote the user's words for the destination and the goal behind it verbatim from the ledger; never restate them in your own words.

```text
🧭 **Drift check** (after round N)

Destination and goal, in your words: "<verbatim quote>"

Decided by you: D1, C1 - <one-line gists>
Taken from my recommendations: D2, D5 - <one-line gists, each with the assumption it carries>
Out of scope: ... | Unresolved: ... | Newly inferred: ... | Canonical artifact: <path>

Does this still describe what you want? Check the recommendations above most closely.
```
