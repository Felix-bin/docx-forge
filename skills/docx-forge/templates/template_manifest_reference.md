# Template Manifest Structure

> Grammar of `template.yaml`, the manifest of a template workspace. Machine schema: [`schemas/template.schema.json`](schemas/template.schema.json). Create Template writes it; the checker and compiler read it; the Writer never edits it.

A template workspace is a folder:

```text
<template-id>/
├── reference.docx     # cleaned copy of the user's template; the compiler's mold
├── template.yaml      # this manifest
├── README.md          # usage notes and rules, from the user's README
└── inspection.json    # derived: the ingestion audit report
```

## 1. Complete Example

```yaml
schema_version: 1
id: gov-request
name: Official Request
kind: document                 # format | document
genre: official-request
language: zh-CN
source:
  file: 请示模板.docx
  sha256: "<64 hex>"
  ingested_at: "2026-10-27T10:00:00+08:00"
content_anchor: "{{正文}}"
slots:
  标题:     {type: text, required: true}
  发文字号: {type: text, required: true}
  主送机关: {type: text, required: true}
  日期:     {type: date, required: true}
style_map:
  paragraph:
    h1:             {id: "GovHeading1", name: "公文一级标题"}
    h2:             {id: "GovHeading2", name: "公文二级标题"}
    body:           {id: "GovBody",     name: "公文正文"}
    list_number:    {id: "GovList",     name: "公文列表"}
    caption_table:  {id: "TableTitle",  name: "表题"}
  character:
    strong:         {id: "Strong",      name: "Strong"}
  table:
    table:          {id: "GovTable",    name: "公文表格"}
heading_levels: 4
numbering:                     # informative; read from the linked multilevel list
  h1: "一、"
  h2: "（一）"
  h3: "1."
  h4: "（1）"
captions:
  table:  {label: "表", position: above, include_chapter: false}
  figure: {label: "图", position: below, include_chapter: false}
sections:
  - {name: body, page_number_format: arabic, start_at: 1}
fixed_regions: [issuing_header, signature_block]
limits:
  max_pages: 3
skeleton: []                   # document kind only; optional
```

## 2. Field Contract

| Field | Required | Meaning |
|---|---|---|
| `schema_version` | yes | Always `1` for now |
| `id` | yes | Lowercase hyphenated id, unique in the library index |
| `name`, `genre`, `language` | yes | Display name, free-text genre, BCP 47 document language |
| `kind` | yes | `format` (styles and page setup only) or `document` (adds fixed regions, slots, optional skeleton) |
| `source` | yes | Original file name, its SHA-256, ingestion time; the original is never modified |
| `content_anchor` | yes | Exact text of the anchor paragraph replaced by the compiled body |
| `slots` | no | Slot name → `type` (`text`, `multiline`, `date`), `required`, optional `description` |
| `style_map` | yes | Style role → `{id, name}` where `id` is the Word `w:styleId`; grouped as `paragraph`, `character`, `table`. `h1` and `body` are required; any other role is required only when a section uses it |
| `heading_levels` | yes | Deepest heading level the template supports |
| `numbering` | no | Sample number format per heading level, for typed-number detection and display |
| `captions` | no | Caption label text, position, and whether chapter numbers are included, per object kind |
| `sections` | yes | Page-numbering behavior of each Word section in body order |
| `fixed_regions` | no | Names of regions kept verbatim |
| `limits` | no | Optional `max_pages` (checked in exact preview) |
| `skeleton` | no | `document` kind only: ordered `{title, level, required, guidance}` entries the Planner must honor |

## 3. Boundary

- A style role absent from `style_map` cannot be used; add it by re-running Create Template, never by editing generated documents.
- `style_map` values point to styles that exist in `reference.docx`; ingestion verifies this and the checker re-verifies the fingerprint.
- `numbering` is descriptive; numbers are produced only by the multilevel list linked inside `reference.docx`.
