# Visual Surface Map

## When to Use

Use this skill when you need to understand where and how a domain concept surfaces across the UI. Triggers include:

- "where does X appear in the UI?"
- "how is X exposed visually?"
- "map the visual surfaces for X"
- "what can users do with X in the UI?"
- Before making changes to a domain entity and needing to understand the visual blast radius

This skill covers visual interfaces only — screens, pages, views across web, mobile, or desktop. It does not cover non-visual interfaces like REST APIs, GraphQL schemas, or MCP servers.

## Inputs

Gather from the user:

1. **Domain concept** (required) — an entity, feature, or domain area (e.g. `Invoice`, `User`, `Notification`)
2. **Codebase path** (required) — path to the codebase on disk
3. **Entity artefact path** (optional) — used to bootstrap discovery from known modules and files

## Output

This skill does NOT produce a persisted artefact. It outputs its findings directly into the conversation to inform the next task.

## Prompt

Read and follow the prompt at: `visual-surface-map/PROMPT.md`
