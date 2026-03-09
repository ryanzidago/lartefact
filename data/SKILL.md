---
name: lartefact-data
description: Use when asked to compile a data profile, profile a database table, or understand live data for a component — "compile data profile for X", "profile the X table", "what does X data look like?"
---

# Data Profile Artefact

## Triggers

- "compile data profile for X"
- "profile the X table"
- "what does X data look like?"

## Inputs

1. **Component name** (required)
2. **Code artefact path** (recommended) — path to an existing code artefact, used to bootstrap table name, schema, relationships, and indexes
3. **Codebase path** (fallback) — used to discover schema if no code artefact is provided. Defaults to current working directory.
4. **Output path** (optional) — where to write the artefact. If not provided, ask.

## Output

`{component_name}_data_profile.md`

## Prompt

Read and follow `PROMPT.md` in this directory.
