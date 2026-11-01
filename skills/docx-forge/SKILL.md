---
name: docx-forge
description: >
  Template-driven workflow for writing natively styled, editable Word documents.
  Plans the argument first, writes constrained DocMD sections, and compiles them
  into the user's own Word template using only template styles, generated
  numbering, captions, cross-references, and a table of contents. Also ingests a
  user's .docx/.dotx into a clean reusable template workspace, with a live
  browser preview of the compiled document. Use when the user asks to write,
  draft, generate, or fill a Word document, DOCX, report, proposal, official
  document, or thesis from a template, to prepare or register a Word template,
  or mentions docx-forge.
metadata:
  version: "0.0.1"
  license: "MIT"
---

# docx-forge Skill

docx-forge is a routed document workflow. This entry owns global execution discipline and route selection only; each selected route owns its procedure.

## Mandatory Load Order

**Hard rule — paths before commands**: retain the host-provided absolute directory containing this file as `SKILL_DIR`. Per tool call, expand `${SKILL_DIR}`. Never `cd` into it, rely on the current directory, or assume a repo checkout. If unavailable, ask; never search or guess.

1. Read this file.
2. Read [`workflows/routing.md`](workflows/routing.md) through `${SKILL_DIR}/workflows/routing.md`.
3. Select exactly one top-level route and its profile from the routing authority.
4. If the selected authority is marked unavailable, state that and stop.
5. Read only the selected runtime authority and its explicitly triggered supporting documents.

**Hard rule — selected authority only**: do not load another top-level route's procedure after routing. Supporting documents refine one route; they never compete with it.

---

## Vocabulary

| Term | Meaning |
|---|---|
| **DocMD** | The project-canonical authoring language: constrained Markdown with directive blocks, contract in [`references/docmd-core.md`](references/docmd-core.md) |
| **Section file** | One `sections/NN_<slug>.md` file holding one top-level section; the unit of serial writing |
| **Block** | One compiled body unit (paragraph, list item, table, figure) with a stable id `s<NN>.b<k>` used by the preview and annotations |
| **Label** | A semantic id such as `{#tbl:budget}`; the compiler turns it into a bookmark plus generated number |
| **Style role** | A semantic name (`h1`, `body`, `caption_table`, `strong`, …) that a template's `style_map` binds to a real Word style |
| **Template workspace** | A cleaned, ingested template: `reference.docx` + `template.yaml` + `README.md`; grammar in [`templates/template_manifest_reference.md`](templates/template_manifest_reference.md) |
| **`format` / `document` template** | Styles and page setup only, versus styles plus fixed regions, slots, and an optional skeleton |
| **Slot** | A `{{name}}` placeholder or tagged content control in a fixed region, filled from `slots.yaml` |
| **Content anchor** | The template paragraph (usually `{{正文}}`) replaced by the compiled body |
| **Ledger** | `analysis/ledger.md`: abbreviations, labels, and per-section conclusions carried across sections of a long document |

## Global Execution Discipline

1. **Serial execution** — follow the selected authority's steps in order.
2. **Blocking means stop** — at every `⛔ BLOCKING` gate, wait for explicit user confirmation; do not decide on the user's behalf.
3. **No cross-phase bundling** — do not combine work across an unclosed gate.
4. **Gate before entry** — verify every listed prerequisite before entering a step.
5. **No speculative execution** — do not prepare later-phase artifacts before their owning step.
6. **Deterministic routing** — do not add a route-choice question when [`routing.md`](workflows/routing.md) resolves the request.
7. **Act at the owning layer** — repair a section-local issue in the section, a roster or evidence fault in the plan, a style fault in the template workspace; then resume.
8. **Hand-write sections** — the main agent writes section files one at a time; never generate prose with a script.
9. **Template boundary** — every compiled paragraph, run, and table references a style the template's `style_map` declares; never create a style, a numbering scheme, or direct formatting. A missing style role returns to Create Template, never to the section.

## Global Communication Rules

- Write the document in the source or user language unless the user overrides it; reply in the user's chat language.
- Localize user-facing option labels and explanations; keep exact enum ids and field names when precision needs them.
- Keep `doc_spec.md` and `doc_lock.md` headings and field names in English; values may use the document language.
- Before switching roles, read the role reference and output:

```markdown
## [Role Switch: <Role Name>]
📖 Reading role definition: references/<filename>.md
📋 Current task: <brief description>
```

## Repository Compatibility

- This package is a workflow skill, not an application scaffold.
- Keep every runtime workflow, reference, script, and template document inside this skill directory; package files must not depend on repository-level instructions.
- On Windows, if `python3` is unavailable, rerun the same command with `python`.
