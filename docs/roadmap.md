# Roadmap

[English](./roadmap.md) | [Chinese](./zh/roadmap.md)

---

Phases are ordered narrow-then-deep. Each phase ships only when its acceptance criteria pass.

| Phase | Scope | Acceptance criteria |
|---|---|---|
| **P0 — Charter** | Positioning, technical design, template preparation guide, DocMD contract draft, template manifest grammar and schema, `SKILL.md` and `routing.md` skeletons, repo rules | The user can prepare templates from the guide; every document cross-link resolves |
| **P1 — Template ingestion** | `template_inspect.py` audit report; Create Template writes a clean workspace (`reference.docx` + `template.yaml` + `README.md`); docx-preview block-map spike | On one or two of the user's real templates, the report correctly names defects and the workspace validates against the schema |
| **P2 — Quick generate + live preview** | `docmd_to_docx.py` (style map, slots, numbered headings, lists, tables with repeating header, figures with captions, footnotes, TOC field, cross-references); `docmd_checker.py`; Quick profile; read-only live preview that refreshes per section | Output uses only template styles with zero direct formatting; numbering and fields are correct after "Update Field" in Word; the preview refreshes as each section is written |
| **P3 — Default route** | Planner role with two-stage confirmation (Stage 1 includes template choice); `doc_spec.md` / `doc_lock.md` / ledger; early and final gates; citations and bibliography; preview annotations and apply loop; Word COM exact preview | A 10,000+ word document keeps consistent terms and voice; every citation resolves; annotate → apply → re-export works end to end |
| **P4 — Edit Native DOCX** | Round-trip workspace for an existing docx: fill, revise selected sections, tracked changes, comments | Unchanged paragraphs keep byte-identical XML |
| **Later** | OMML equations, native charts, content controls as structured input, multi-section appendices | Admitted one by one under the capability admission test |
