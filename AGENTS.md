# AGENTS.md

This file is the project entry point for general AI agents.

**You MUST read [`skills/docx-forge/SKILL.md`](skills/docx-forge/SKILL.md) before any document generation task or repo modification.** It owns global execution discipline and points to the route selector; after routing, the selected runtime authority owns its steps, gates, and commands.

## Project Overview

docx-forge turns source material into natively styled, editable Word documents by compiling a constrained authoring language (DocMD) **into the user's own Word templates**. The template is the mold: it owns styles, numbering, page setup, headers/footers, and fixed regions; the compiler only inserts content that references template styles.

**Route selection authority**: [`skills/docx-forge/workflows/routing.md`](skills/docx-forge/workflows/routing.md) owns the three top-level routes: Generate DOCX, Create Template, and Edit Native DOCX.

**Project status**: P0 (charter and scaffold). Route authorities, scripts, and the live preview are not built yet; see [`docs/roadmap.md`](docs/roadmap.md). A route whose authority does not exist yet is unavailable — state that and stop.

## Required Conventions

- **Repo-wide rules** — follow [`docs/rules/`](docs/rules/) when editing any prompt file, document, or (later) Python script.
- **Prompt content layers** — [`docs/rules/prompt-layers.md`](docs/rules/prompt-layers.md): a prompt file holds craft and minimal contract; procedure lives in the owning workflow; tool behavior lives in `skills/docx-forge/scripts/docs/`.
- **Decision ownership** — [`docs/rules/ownership.md`](docs/rules/ownership.md); every cross-file rule has one owner recorded in [`docs/rules/rule-owners.md`](docs/rules/rule-owners.md).
- **Language** — [`docs/rules/language.md`](docs/rules/language.md): English is canonical; `docs/zh/` and `README_CN.md` are synchronized translations updated in the same change.

## Core Directories

- `skills/docx-forge/SKILL.md` — global discipline and route-entry authority.
- `skills/docx-forge/workflows/` — route authorities, profiles, and stages.
- `skills/docx-forge/references/` — role cores and the DocMD language contract.
- `skills/docx-forge/templates/` — template manifest grammar, schemas, and (later) registered template workspaces.
- `docs/` — positioning, technical design, template preparation guide, roadmap.
- `docs/rules/` — repo-wide rules.
- `projects/` — user project workspace (ignored by git).
