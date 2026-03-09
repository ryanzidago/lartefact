---
description: Use when asked to compile a code artefact, document a component's structure, or analyze code — "compile code artefact for X", "document X code", "document the X component"
---

# Code Artefact

## Triggers

- "compile code artefact for X"
- "document X code"
- "document the X component"

## Inputs

1. **Component name** (required) — e.g. `User`, `Order`, `HolidayCalculator`
2. **Codebase path** (optional) — defaults to current working directory
3. **Output path** (optional) — where to write the artefact. If not provided, ask.

## Output

`{component_name}.md`

## Prompt

Read and follow `PROMPT.md` in this directory.
