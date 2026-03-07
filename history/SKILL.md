# Entity History Artefact Compiler

## When to Use

Use this skill when asked to compile, generate, or create a history artefact for an entity. Triggers include:

- "compile history artefact for X"
- "create a history artefact for the X entity"
- "who owns the X entity?"
- "what's the history of X?"

## Inputs

Gather from the user:

1. **Entity name** (required) — e.g. `User`, `Order`, `Invoice`
2. **Codebase path** (required) — path to the codebase on disk
3. **Entity artefact path** (optional) — path to an existing entity artefact for this entity, used to bootstrap file discovery
4. **Output path** (optional) — where to write the artefact. If not provided, ask.

## Output

A single markdown file: `{entity_name}_history.md`

## Prompt

Read and follow the prompt at: `history/PROMPT.md`