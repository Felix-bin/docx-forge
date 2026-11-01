# Template Preparation Guide

[English](./template-preparation-guide.md) | [Chinese](./zh/template-preparation-guide.md)

---

docx-forge compiles content **into your Word templates**. A template prepared along these lines ingests cleanly; one that relies on manual formatting will produce an audit report full of `needs-manual` items. Ingestion never modifies your original file, so you can prepare templates incrementally.

## 1. Folder Layout

Keep one folder per template:

```text
my-templates/
└── company-report/
    ├── company-report.docx      # or .dotx
    ├── README.md                # purpose and rules (see §6)
    └── sample.docx              # optional: a real filled example
```

## 2. Use Styles for Everything Structural

| Content | Do | Do not |
|---|---|---|
| Headings | Apply `Heading 1`…`Heading 4` or your own named heading styles | Bold text with a larger font |
| Body text | One body style, for example `Body Text` or a custom `正文` style | `Normal` with manual indents and spacing |
| Bulleted / numbered lists | A list style or paragraph style bound to a list | Typed `•` or `1.` characters |
| Table and figure captions | `Caption` or dedicated caption styles | A centered bold paragraph |
| Tables | A table style (header row, borders, shading) | Manually bordered cells |
| Quotes, notes, warnings | Dedicated paragraph styles | Italic or shaded body paragraphs |

**Tip**: in Word, open the Styles pane with "Show Preview" off and "Style Inspector" on; every structural paragraph should show a named style and no "plus" direct formatting.

## 3. Link Heading Numbering to Heading Styles

If your headings are numbered (for example `一、` / `（一）` / `1.` / `（1）`, or `1` / `1.1` / `1.1.1`):

1. Home → Multilevel List → **Define New Multilevel List**.
2. For each level, set the number format and click **More** → **Link level to style** → choose the matching heading style.
3. Never type the number into the heading text.

Ingestion reads this binding; typed numbers are reported as defects.

## 4. Mark Where the Body Goes

Put a single paragraph containing exactly `{{正文}}` (or `{{body}}`) where generated content should be inserted. Everything before and after it — cover, issuing header, signature block, appendices — is treated as a fixed region and kept as is.

For a `format` template with no fixed regions, the anchor may be the only paragraph in the body.

## 5. Mark Variable Fields

For values that change per document (title, date, document number, recipient), use either:

- a placeholder `{{field_name}}` typed as plain text inside the paragraph that should hold it, for example `{{标题}}`, `{{发文字号}}`, `{{日期}}`; or
- a Word content control (Developer → Plain Text Content Control) whose **Tag** is the field name.

Keep each placeholder inside one run: type it in one go without changing formatting mid-word.

## 6. Write a README Next to Each Template

```markdown
# Company Report Template
- Use for: quarterly and project reports to management
- Genre: report
- Fixed regions: cover page, confidentiality footer
- Fields: {{标题}}, {{部门}}, {{日期}}
- Rules: no more than 3 heading levels; every table needs a caption
- Do not change: cover layout, footer text
```

The README becomes the template's usage notes; rules written here are honored during planning.

## 7. Page Setup and Sections

- Set margins, paper size, and page-number formats in the template itself.
- If front matter uses Roman page numbers and the body uses Arabic, separate them with a **section break** in the template and set "start at 1" in the body section.
- Headers and footers that differ per section should be set up in the template; the compiler does not create them.

## 8. Checklist Before Handing a Template Over

- [ ] Every heading uses a heading style; no typed numbers
- [ ] Multilevel list levels are linked to heading styles
- [ ] Body, list, caption, table, and quote styles exist
- [ ] Exactly one `{{正文}}` anchor paragraph
- [ ] Variable fields are `{{field}}` placeholders or tagged content controls
- [ ] README written
- [ ] Optional: a filled sample document for reference
