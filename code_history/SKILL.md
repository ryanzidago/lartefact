---
name: lartefact-code-history
description: Use when asked to compile a history artefact, determine component ownership, or understand change history — "compile history artefact for X", "who owns X?", "what's the history of X?"
---

# Code History Artefact

## Triggers

- "compile history artefact for X"
- "who owns X?"
- "what's the history of X?"

## Inputs

1. **Component name** (required)
2. **Codebase path** (optional) — defaults to current working directory; must be a git repository
3. **Code artefact path** (optional) — path to an existing code artefact, used to bootstrap file discovery. If not provided, discover files independently.
4. **Output path** (optional) — where to write the artefact. If not provided, ask.

## Output

`{component_name}_history.md`

## Prompt

Read and follow `PROMPT.md` in this directory.
