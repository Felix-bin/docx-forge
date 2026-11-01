# Language Rule

> Applies to every agent-facing file — `skills/docx-forge/SKILL.md`, `references/`, `workflows/`, `templates/*.md` — and to repository documentation under `docs/`.

## 1. One Language Per File

Each Markdown file is single-language. English is canonical. `README_CN.md` and every file under `docs/zh/` are complete Chinese translations of their English counterparts, updated in the same change; a translation never carries a rule the English file lacks.

Runtime files under `skills/docx-forge/` and rule files under `docs/rules/` are English only.

## 2. A Non-English String May Be Content, Never a Rule

Inside an English file, a non-English string is allowed only when it *is* the subject matter.

| Allowed — the string is content | Forbidden — the string is the rule |
|---|---|
| A placeholder the user types into a template (`{{正文}}`, `{{发文字号}}`) | A decision rule written in Chinese |
| A numbering format sample (`一、`, `（一）`) | A selection criterion phrased in Chinese |
| A style name that genuinely exists in a template (`公文正文`) | A section heading or table header in Chinese |
| A sample field value in an example block | A prose paragraph of instructions in Chinese |

**Hard rule**: an instruction the model must follow is written in the file's own language. A rule written in another language biases output toward that language.

## 3. Never Hard-Code the Output Language

Do not write a rule that fixes which language the model writes the document or replies in. Say "in the user's language" or "in the source language unless the user overrides it". A fixed message template written in one language carries a note to translate it into the user's chat language.

## 4. Trigger Text Stays Language-Neutral

`SKILL.md`'s `description` is English prose. Do not append per-language keyword lists; widen the English intent vocabulary instead (including language-neutral tokens such as `DOCX` and `Word`).
