# Entity Artefact Compiler

## When to Use

Use this skill when asked to compile, generate, or create an entity artefact. Triggers include:

- "compile entity artefact for X"
- "create an artefact for the X entity"
- "document the X entity"

## Inputs

Gather from the user:

1. **Entity name** (required) — e.g. `User`, `Order`, `Invoice`
2. **Codebase path** (required) — path to the codebase on disk
3. **Output path** (optional) — where to write the artefact. If not provided, ask.

## Output

A single markdown file: `{entity_name}.md`

## Prompt

Read and follow the prompt at: `entity/PROMPT.md`