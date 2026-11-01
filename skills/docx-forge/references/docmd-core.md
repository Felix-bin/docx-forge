# DocMD Core

> Status: P0 draft. The contract the Writer authors against and `docmd_checker.py` will enforce. Constructs map one-to-one onto native Word mechanisms through the template's `style_map` ([`template_manifest_reference.md`](../templates/template_manifest_reference.md)).

DocMD is constrained Markdown. Write **semantics only**: never type a number, a caption prefix, a table of contents, or any formatting. Everything Word can generate, the compiler generates.

---

## 1. Construct Recall

| Construct | Canonical form | Style role | Word mechanism |
|---|---|---|---|
| Heading | `## Title {#sec:id}` (label optional) | `h1`…`h6` by depth | Heading style linked to the template's multilevel list; label → bookmark |
| Paragraph | Plain text block | `body` | Paragraph with the body style |
| Strong / emphasis | `**text**` / `*text*` | `strong` / `emphasis` | Character style, never direct bold or italic |
| Inline code | `` `text` `` | `code_inline` | Character style |
| Hyperlink | `[text](https://…)` | `hyperlink` | Hyperlink relationship + character style |
| Bulleted list | `- item` (nest with two spaces, max 3 levels) | `list_bullet` | Paragraph style bound to the template's bullet list |
| Numbered list | `1. item` (always `1.`) | `list_number` | Paragraph style bound to the template's numbered list; numbers generated |
| Styled block | `::: quote` … `:::` | the directive name (`quote`, `note`, `warning`, or any paragraph role the template declares) | Paragraphs with that style |
| Table | `::: table {#tbl:id caption="…"}` + pipe table + `:::` | `table`, `caption_table` | Table style, repeating header row, caption with `SEQ` field, bookmark |
| Figure | `::: figure {#fig:id src="assets/…" caption="…"}` + `:::` | `figure`, `caption_figure` | Inline picture paragraph, caption with `SEQ` field, bookmark |
| Cross-reference | `[@sec:id]`, `[@tbl:id]`, `[@fig:id]` | — | `REF` field to the label's bookmark |
| Citation | `[@cite:key]` | `bibliography` for the list | Key resolved against `references.bib`; rendered in the locked citation style |
| Footnote | `text[^n]` + `[^n]: note` at the end of the file | template footnote styles | Native footnote |
| Table of contents | `::: toc {levels="1-3"}` + `:::` | template TOC styles | `TOC` field (page numbers filled by Word) |
| Page break | `::: pagebreak` + `:::` | — | Page break run |

A directive opens with `::: name {attributes}` on its own line and closes with `:::` on its own line. Attribute values are double-quoted.

## 2. Examples

```markdown
## Budget Overview {#sec:budget}

The total budget is shown in [@tbl:budget]; the method follows [@cite:gb2020].[^1]

::: table {#tbl:budget caption="Budget breakdown" header-rows="1" widths="30,70"}
| Item | Amount (10k CNY) |
|---|---|
| Equipment | 120 |
:::

::: figure {#fig:arch src="assets/arch.png" caption="System architecture" width="80%"}
:::

[^1]: Source: 2025 final accounts.
```

## 3. Writing Craft Hooks

- **Headings carry the argument**: a heading states what the section establishes, not a topic word, unless the genre fixes headings.
- **Prefer prose; use a list for parallel items, a table for items compared on the same attributes.** A table with one data column is a list.
- **Every table and figure is cited** at least once in the text before it appears.
- **Labels are semantic and stable**: `tbl:budget`, never `tbl:3`; renaming a label renames every reference.

## 4. Input States

| State | Examples | Checker |
|---|---|---|
| Canonical | Everything in §1 | Pass |
| Compatible (normalized) | `> quote` → `::: quote`; `__strong__` → `**strong**`; `* item` → `- item`; `2.` list markers → `1.` | Warning |
| Invalid | See §5 | Error, blocks export |

## 5. Boundary

- **Forbidden — typed numbering**: heading text starting with a number pattern (`1.2 `, `一、`, `（一）`, `第三章`), caption text starting with `Table 3` / `表3`, hand-written contents lists.
- **Forbidden — formatting**: raw HTML, inline styles, `<br>`, a bold-only paragraph used as a heading, setext headings, horizontal rules.
- **Forbidden — bare objects**: a pipe table outside `::: table`, Markdown image syntax `![…](…)`, a figure `src` outside `assets/`.
- **Forbidden — unresolved references**: a `[@…]` whose label or citation key does not exist; a duplicate label.
- **Forbidden — unmapped roles**: any construct whose style role is absent from the template's `style_map`; the fix belongs to the template workspace.
- **Reserved — formulas**: `$…$` and `$$…$$` are rejected until OMML support ships.
