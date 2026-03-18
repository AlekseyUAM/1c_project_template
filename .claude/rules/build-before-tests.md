# Build Before Tests — Project Rule

> RULES — mandatory project-level policy. Applies to all agents working with tests in this project.

---

## Purpose

After writing or modifying any BSL/XML file in the project, the extension MUST be loaded
into the information base (ИБ) via `build_project` before running tests.
Without this step, `run_tests` executes against stale code in the ИБ.

---

## Rule

### MUST (mandatory)

| # | Requirement |
|---|-------------|
| 1 | After ANY change to `.bsl` or `.xml` files in `src/cf/` or `src/cfe/` — call `build_project` BEFORE `run_tests` |
| 2 | Do NOT skip `build_project` even if only one line was changed |
| 3 | If `build_project` returns "Исходные файлы не изменены" — verify that the file was actually saved to disk before the call |
| 4 | If `build_project` fails — do NOT proceed to `run_tests`; diagnose the build error first |

### Workflow

```
1. Write/edit .bsl or .xml files
2. build_project          ← MANDATORY: loads config + extensions into ИБ (UpdateDBCfg)
3. run_tests / run_module_tests  ← only after successful build
```

### What `build_project` does in this project

`build_project` performs the following operations for each source in `source-set`:

1. `/LoadConfigFromFiles src/cf` — loads main configuration
2. `/LoadConfigFromFiles src/cfe/unit_tests -Extension unit_tests` — loads test extension
3. `/UpdateDBCfg` — applies all changes to the information base

Without step 2+3, the ИБ does not know about new or modified test modules.

---

## Common mistakes

| Mistake | Consequence |
|---------|-------------|
| Skipping `build_project` after writing tests | `run_tests` runs old code, tests pass/fail incorrectly |
| Calling `run_tests` immediately after file edit | Same as above — ИБ has stale extension |
| Ignoring "Исходные файлы не изменены" | File may not have been flushed to disk; verify with Read |

---

## Scope

This rule applies to:
- Developer-Tests (Phase 3a)
- Developer-Code (Phase 3b)
- Tester (Phase 4)
- Any agent running tests in quick-fix workflow
