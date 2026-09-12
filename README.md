# Grilling Skill

A persistent, round-based interview for stress-testing plans, decisions, architectures, and ideas.

Unlike a context-only question loop, this Skill records the decision tree, confirmed requirements,
open questions, assumptions, conflicts, and the current frontier in a `.grilling/` ledger so a long
conversation can resume after context compaction.

## Install

```bash
npx skills add a1024053774/grilling-skill@grilling -g -y
```

Or clone this repository and link `grilling/` into the Skill directory used by your Agent.

## Use

Ask an Agent to grill a plan or idea, for example:

```text
Use the grilling skill to stress-test this architecture: ...
```

The active ledger is created in the current workspace at `.grilling/ACTIVE.md` and
`.grilling/<topic-slug>.md`.

## License

MIT. See [LICENSE](LICENSE).
