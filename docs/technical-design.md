# Technical Design

[English](./technical-design.md) | [Chinese](./zh/technical-design.md)

---

## 1. Design Philosophy — AI-Directed Workflow, Human-Controlled Draft

The workflow reasons about the document, writes it section by section, and compiles it into the user's template under an explicit route contract. Deterministic tools convert, compile, validate, and preview; they never make design or wording decisions. The user reviews the direction and owns the last mile in Word.

Three layers have separate responsibilities and cannot substitute for one another:

| Layer | Responsibility | Not responsible for |
|---|---|---|
| Workflow | Sequence steps, gates, role switches, and handoffs | Planner, Writer, checker, or compiler decisions |
| Prompts and examples | State the canonical DocMD spelling and the writing craft | Correctness or safety — they are not a boundary |
| `docmd_checker.py` | Enforce the DocMD contract and template bindings; errors block, warnings pass | Rewriting sources or guessing intent |
| `docmd_to_docx.py` | Compile canonical DocMD into the template; require a matching final checker report | Repairing sources or rerunning the checker |

## 2. Why a Flow Markup Language, Not SVG

PPT Master chose SVG by elimination: PowerPoint is an absolute-positioned canvas, and SVG shares that world view, while HTML's flowing document model does not. Word is the opposite case. A Word document is a **flow** of paragraphs, runs, lists, tables, and sections whose positions are determined by content and styles. The same reasoning therefore selects a flow markup:

| Candidate | Verdict |
|---|---|
| Direct WordprocessingML | Too verbose and fragile for the model to author; impossible to review by eye |
| Free Markdown or HTML | Right world view, but open: the model writes inline styling and manual numbering, and nothing can be closed or checked |
| SVG / absolute layout | Wrong world view |
| **DocMD — constrained Markdown with directive blocks** | The model knows it; humans read and diff it; the grammar is closed; every construct maps to one native Word mechanism |

DocMD is a project-canonical language, not "Markdown support". The author writes **semantic labels only**; the compiler generates every number and field. The contract lives in [`docmd-core.md`](../skills/docx-forge/references/docmd-core.md).

| Typical model failure in Word | Owner of the fix |
|---|---|
| Fake headings (bold + larger font) | Checker rejects inline formatting; headings are `#` only and map to template heading styles |
| Hand-typed `1.1` / `Figure 3`, then wrong numbers | Compiler numbers through multilevel lists and `SEQ` fields; the author writes `{#fig:x}` |
| Hand-written table of contents | Compiler inserts a `TOC` field |
| Inconsistent terms and abbreviations | Term list in `doc_lock.md` plus checker warnings |
| Invented citations | Citation keys must resolve to `references.bib` or a source; otherwise error |
| Style drift and repetition late in a long document | Serial section writing, an early gate after the sample sections, and a running ledger |

## 3. The Template Is the Mold

The compiler opens a copy of the template workspace's `reference.docx`, fills declared slots, removes the content-anchor paragraph, and inserts compiled blocks there. The result inherits the template's `styles.xml`, `numbering.xml`, theme, settings, section properties, headers, footers, and fixed regions unchanged.

**Invariant — no new styles, no direct formatting**: every inserted paragraph, run, and table references a style that the template's `style_map` declares. A DocMD construct whose style role is missing from the map is a checker error that points to the template, never a reason to synthesize formatting.

Two template kinds exist (grammar: [`template_manifest_reference.md`](../skills/docx-forge/templates/template_manifest_reference.md)):

| Kind | Holds | Example |
|---|---|---|
| `format` | Styles, numbering, page setup, headers/footers; the whole body is planned | Company report format, thesis format |
| `document` | `format` plus fixed regions, slots, and an optional section skeleton | Official request, project application form, weekly report |

**Ingestion before use**: a raw user template is never consumed directly. Create Template audits it (fake headings, manual numbering, unlinked multilevel lists, missing caption/table styles, direct formatting, unused styles), then writes a **new** clean workspace. The original file is never modified. Defects the tool cannot repair safely are reported as `needs-manual`.

## 4. Artifact Flow

```text
sources/<files> ─────────┐
analysis/source_profile ─┼─> Planner -> doc_spec.md + doc_lock.md
templates/<installed>/ ──┘
                                   │
doc_spec.md + doc_lock.md + analysis/ledger.md + assets/ + references.bib
    └─> Writer -> sections/NN_<slug>.md            (authored state)
          ├─> docmd_checker.py -> validation/quality_report.json
          └─> docmd_to_docx.py -> exports/<name>_<ts>.docx + validation/<stem>.report.json
                                  preview/current.docx + preview/blockmap.json   (live preview)
```

`sections/` is authored state. `preview/`, `exports/`, and `validation/` are derived. Deleting that distinction makes re-export and repair impossible to reason about.

**Long documents**: a report can exceed one context window. After each section the Writer appends to `analysis/ledger.md` — defined abbreviations, labels introduced, and a one- or two-sentence summary of what the section concluded. The next section reads the spec, lock, and ledger, never the full prior text.

## 5. Plan and Execution Ownership

| Role | Owns |
|---|---|
| User | Facts, sources, template choice, desired outcome, exclusions |
| Planner | Reader, purpose, core conclusion, genre, outline, per-section obligations and evidence, target lengths, term list, citation style; writes `doc_spec.md` (human-readable rationale) and `doc_lock.md` (machine-readable anchors) |
| Writer | Prose, paragraphing, emphasis, table and figure realization inside the planned obligations; uses only prepared assets |
| Checker / Compiler | Deterministic enforcement and packaging |

The detailed tiers are owned by [`ownership.md`](./rules/ownership.md).

## 6. Quality Gate

**Severity**: `error` blocks (broken structure, unresolved label or citation, unmapped style role, missing asset, invalid construct); `warning` passes (non-canonical but safely normalizable spelling, mixed full/half-width punctuation, a section far from its target length, a term outside the term list).

**No auto-fix**: a mechanical patch can overwrite valid intent. The checker reports; the Writer repairs at the owning layer.

**Cadence**: the first one or two sections are a method sample followed by an early gate; the remaining sections are written continuously; a final gate inspects the complete set. The compiler refuses a missing, stale, or blocking final report (matched by a fingerprint of `sections/`).

## 7. Live Preview

PPT Master previews its authored SVG because SVG is the visual truth. In docx-forge the visual result comes from the template plus the compiler, so the preview **must show the compiled document** while every block stays traceable to its source.

| Layer | Mechanism | Latency | Purpose |
|---|---|---|---|
| Live | The preview server watches `sections/*.md`, recompiles `preview/current.docx` after a debounce, and the browser renders it with docx-preview.js | ~1 s | Follow writing as it happens with real template styles, headers, footers, and cover; pagination approximate |
| Exact | Word COM opens the docx, updates all fields, and exports PDF; the browser shows it with pdf.js | seconds | Real page count, table-of-contents page numbers, final layout |

**Block map**: each compile writes `preview/blockmap.json` mapping the n-th body block (paragraph or table) to `sections/<file>` line range and a stable block id such as `s03.b12`. The front end resolves a click to a source block by order. This mapping is validated by a P1 spike; the fallback is a per-block bookmark.

**Editing surface**: annotations (instructions that need the model) are stored in a sidecar `live_preview/annotations.json` with the block id and the quoted text, never as markers inside Markdown. Direct edits are limited to wording and to switching a block's style role among the template's declared roles; font, size, and color controls are deliberately absent because they would be direct formatting.

**Lifecycle** follows PPT Master's editor conventions: a single instance per project recorded in `live_preview/lock.json`, the first free port from 6060, an idle timeout, draft persistence with digest-based conflict detection, and `--live --daemon` startup before the first section. Annotations submitted during writing are applied only after export.

## 8. Architecture Invariants

1. Never insert a style or direct formatting that the template does not declare.
2. Never modify the user's original template or source document in place.
3. Every number, caption, cross-reference, and table of contents is generated by Word mechanisms, never typed.
4. Keep source, authored, derived, and delivery artifacts distinguishable.
5. Select a route and its mutation contract before changing any artifact.
6. Fail closed when a construct cannot be represented safely.
7. Sections are hand-written by the main agent one at a time; never script-generate prose.
