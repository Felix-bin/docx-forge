# Prompt Content Layers

> Which content belongs in a prompt file at all, and where each kind lives. Applies to every runtime-loaded file: `SKILL.md`, `skills/docx-forge/references/`, `skills/docx-forge/workflows/`, `skills/docx-forge/templates/*.md`, and `AGENTS.md`.

Prompt files are read by a model before it plans and hand-writes a document. Everything in them competes for attention with the decisions it is about to make. A paragraph earns its place only by being craft, minimal contract, or the owning statement of a procedure.

## 1. The Four Kinds

| Kind | Test | Lives in | Example |
|---|---|---|---|
| **Craft** — writing and structuring judgment | Changing it changes what the document says or how it reads | The prompt file of the role that makes the decision, once | Genre conventions, how to open a conclusion-first report, when a table beats prose |
| **Contract** — minimal form | The one canonical form the model must write, plus a boundary the tools cannot enforce | Beside the craft that uses it, as one example and one line | One DocMD example per construct |
| **Procedure** — what to run, in what order, with which gate | A step, command, gate, or checkpoint the model executes rather than judges | The route authority or stage that owns the step, once; everything else points to it | `project_manager.py init`, the early and final gate cadence |
| **Tool documentation** — checker, compiler, and server behavior | The tool already enforces it, or it describes normalization or lifecycle | `skills/docx-forge/scripts/docs/`, never a prompt file | Accepted-but-warned spellings, preview server lifecycle, report schemas |

**Hard rule — enforced grammar is not prose**: a rule that `docmd_checker.py` already rejects needs nothing in the prompt beyond its canonical form. The failing check teaches the boundary at the moment it matters.

**Hard rule — a selection cue stays with the craft**: when a contract paragraph also says *when to choose* one construct over another, that sentence is craft and stays in the prompt file even if the rest moves.

## 2. Ordering and Ownership Inside Files

**Hard rule — recall first, craft second, boundary last**: open each file and section with the recall index (what exists, the canonical form), then the craft that uses it, then the boundary the tools cannot enforce. A capability the model has not seen is a capability it will not use; a prohibition read first crowds the capability out. Plain Markdown behavior gets one line or nothing — record only where DocMD departs from it.

**Hard rule — one owner, pointers elsewhere**: a rule is stated in full in exactly one file, recorded in [`rule-owners.md`](./rule-owners.md). Every other file carries a pointer naming the file and section, never a paraphrase. A paraphrase is a second owner that drifts.

**Hard rule — planning reads everything, writing loads on triggers**: the Planner reads its complete bundle. The Writer core stays resident and carries a one-line recall of every deeper module (tables, figures, citations, formulas); each deeper module loads on one observable trigger evaluated once over the outline before the first section, is read completely, and stays for the run.

## 3. What This Rule Does Not Permit

- Deleting craft to hit a size target; move the other kinds instead.
- Compressing several rules into one sentence with nested exceptions.
- Restating a moved rule "briefly" in the file it left.
