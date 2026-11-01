# Project Rules

Conventions for contributors and AI agents working in this repository.

| Rule | Scope |
|---|---|
| [`prompt-layers.md`](./prompt-layers.md) | What belongs in a prompt file — craft, minimal contract, procedure, or tool documentation — and where each kind lives |
| [`ownership.md`](./ownership.md) | Which role decides what — plan / execution / Reference tiers, the template boundary, the core volume ceiling |
| [`rule-owners.md`](./rule-owners.md) | Registry of every cross-file rule: its one owner section and the files that may only point to it |
| [`language.md`](./language.md) | One language per file, English canonical with synchronized Chinese translations, no hard-coded output language |

When adding a rule file:

- One topic per file, named `<topic>.md` (lowercase, hyphenated).
- Add a row to the table above.
- Write it **prescriptively** — tell readers what to do, not what the project happens to look like.
- A Python code-style rule is added in P1 together with the first script.
