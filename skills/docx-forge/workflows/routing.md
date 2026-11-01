---
description: Deterministic selection among docx-forge's three top-level routes.
---

# Routing Rules

Route selection authority for docx-forge. Select exactly one top-level route, then activate only the profiles and stages owned by that route.

**Hard rule**: when this file conflicts with a route summary elsewhere, this file wins for route selection. After selection, the active runtime authority owns execution.

---

## 1. Routing Discipline

| Rule | Behavior |
|---|---|
| One artifact lifecycle | Every request enters Generate DOCX, Create Template, or Edit Native DOCX |
| Unavailable authority | A route or profile whose authority is marked unavailable in §2 is stated as not yet built; stop, never improvise its procedure |
| Missing prerequisite | State it and stop that route; do not invent an alternative |
| Ambiguous `.docx` input | Ask one discriminator question only when needed: is it a template to write into (Create Template, then Generate), or a finished document to revise while preserving it (Edit Native DOCX)? |

**Forbidden — route-choice menus**: do not present multiple implementation paths when the request matches one row in §2.

---

## 2. Top-Level Route Matrix

| Route | Request shape | Authority | Status | Preconditions | Output |
|---|---|---|---|---|---|
| Create Template | Prepare, register, or clean a Word template from a `.docx` / `.dotx` | `create-template.md` | Unavailable (P1) | A template file and its README if present | New template workspace; the original file is never modified |
| Generate DOCX — Quick | Write a document into a registered template with explicit quick / fast / skip-planning intent | `profiles/quick-generate.md` | Unavailable (P2) | A registered template workspace; facts exist or research can gather them | `sections/` + DOCX in `exports/` |
| Generate DOCX — Default | Write a document into a registered template | `generate-docx.md` | Unavailable (P3) | Same as Quick | `doc_spec.md` + `doc_lock.md` + `sections/` + DOCX |
| Edit Native DOCX | Fill, revise selected sections of, or annotate an existing finished document while preserving the rest | `edit-native-docx.md` | Unavailable (P4) | Source DOCX exists | New DOCX; unchanged paragraphs byte-identical |

---

## 3. Template Selection Boundary

| User input | Behavior |
|---|---|
| A raw `.docx` / `.dotx` called a template, plus content to write | Create Template first; return to Generate with the new workspace root |
| A registered template id or exact workspace root | Generate with that workspace |
| A bare template name that does not exactly match a registry entry | Never fuzzy-match; list registered templates and ask |
| No template mentioned | Ask which registered template to use; free design is not a route |

Discovery reads only `templates/library/index.json` (created in P1); never scan directories to construct the catalog.

**Hard rule — no in-place template upgrade**: a user template is never edited to add styles, numbering, or anchors. Create Template writes a separate workspace.
