# No Form Changes — Project Rule

> RULES — mandatory project-level policy. Applies to all agents working in this project.

---

## Purpose

Form XML (`Form.xml`) and Data Composition Schemas (СКД/SKD) are generated and managed by the 1C Designer/EDT platform. Direct creation or editing by agents leads to broken forms, invalid UUIDs, layout corruption, and broken data composition. Agents MUST NOT create or modify form XML files or SKD files.

Form **modules** (`Module.bsl`) and report **modules** are regular BSL code and MAY be edited freely.

---

## Rule

### MUST (mandatory)

| # | Requirement |
|---|-------------|
| 1 | Do NOT create, modify, or delete files matching `**/Forms/*/Ext/Form.xml` |
| 2 | Do NOT create form directory structures (`Forms/ФормаДокумента/`, `Forms/ФормаСписка/`, etc.) |
| 3 | Do NOT create, modify, or delete SKD (Data Composition Schema) XML files (`**/DataCompositionSchemes/*/Ext/*.xml` or equivalent in report objects) |
| 4 | Do NOT use `form-dsl`, `xml-gen-cli`, or `skd-dsl` for form/SKD generation unless the user explicitly requests it |
| 5 | If the task requires a new form or SKD — document the requirement in the specification and leave creation to the user |

### Allowed

| # | What is allowed |
|---|-----------------|
| 1 | Editing form module code: `**/Forms/*/Ext/Form/Module.bsl` |
| 2 | Editing report module code (for reports with SKD): `**/Reports/*/Ext/Module.bsl` |
| 3 | Reading `Form.xml` or SKD files for analysis purposes |

---

## What agents SHOULD do instead of creating forms or SKDs

| Need | Action |
|------|--------|
| New document needs a form | Describe required form elements in `technical-design.md`. The user creates the form in Designer/EDT. |
| Existing form needs a new attribute on the UI | Document the change in the spec. The user adds it through Designer/EDT. |
| New report with SKD is needed | Describe report structure, filters, and data sources in `technical-design.md`. The user creates the report and SKD in Designer/EDT. |
| Existing SKD needs a new filter or field | Document the required change in the spec. The user modifies the SKD through Designer/EDT. |

---

## Scope

This rule applies to all agents in all workflows (full-cycle, quick-fix).
