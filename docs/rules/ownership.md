# Decision Ownership: Plan, Execution, and Reference

> Which role decides what, and how strongly a written decision binds. [`prompt-layers.md`](./prompt-layers.md) says what a prompt file may contain; this rule says who owns each decision. Classify a decision here before writing any rule about it.

## 1. The Ownership Chain

| Layer | Owns |
|---|---|
| User / sources / template | Supplied facts, the selected template, desired outcome, exclusions, permissions. The template owns every style, numbering scheme, page setup, header/footer, and fixed region |
| Planner | Reader, purpose, core conclusion, genre, the exact section roster and order, per-section obligations and evidence, target lengths, term list, citation style, slot values; prepares assets and bibliography entries before writing |
| Writer | Prose, paragraphing, emphasis, sentence rhythm, choice between list, table, and prose for one obligation, figure and table realization — inside the planned obligations, using only prepared assets and template style roles |

## 2. Three Tiers

**Hard rule — classify before writing a rule**:

| Tier | Test | Examples | Contract |
|---|---|---|---|
| Plan-only | Holds across the whole document, needs a user confirmation, or comes from source semantics | Section roster and order, core conclusion, evidence per section, terms, citation style, template choice, slot values | The Writer never reopens it; a misfit returns to the plan |
| Execution-only | Judged only while writing the section | Wording, paragraph breaks, local emphasis, list vs. prose for one point, table column order | The plan writes no detail here, not even as advice |
| Reference | Useful as a first sketch, decidable either way | Suggested opening, suggested table for a comparison, tone notes | The Writer adjusts or replaces it freely; it binds only when labeled `(binding)` because the user or template requires it |

**Depth test**: one plan given to two competent Writers yields the same content and structure with different wording. Converging wording means the plan wrote execution; diverging content means it left semantics open.

## 3. The Template Boundary

**Hard rule — the template is not a Writer resource to bend**: neither Planner nor Writer creates a style, a numbering scheme, or direct formatting. A needed style role that the template lacks returns to Create Template, never to the page.

**Hard rule — a template never rewrites the goal**: reader, purpose, and core conclusion come from the user and sources. A template's README rules constrain form (levels, required captions, page limits), not the argument.

## 4. Core Volume Ceiling

**Hard rule**: the always-read Writer core stays small enough that its own content is not diluted. Start with a ceiling of about 300 lines for the Writer role file plus the DocMD core; grow it only when a demonstrated failure requires it, and move deeper material behind a trigger whose one-line recall stays in the core.

## 5. Quick Exception

The Quick profile removes the separate Planner and confirmation handoff: the current agent owns both planning decisions and preparation in one active context, writes no `doc_spec.md` or `doc_lock.md`, and still passes the same checker and compiler. It does not permit changing the template boundary.

> Mnemonic — the template is the mold, the Planner is the foundry plan, the Writer pours the metal. The pour can vary; the mold and the plan cannot be changed mid-pour.
