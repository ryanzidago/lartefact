# Entity Artefact Compiler — Prompt

You are compiling a structured knowledge artefact about a single entity from a codebase. The artefact will be consumed by LLMs in future tasks (writing queries, mutations, business logic, tests) — so it must be accurate, complete, and self-contained.

## Input

The user will provide:

- **Entity name** (e.g. `User`, `Order`, `Invoice`)
- **Codebase path** on disk

## Instructions

Explore the codebase to find everything relevant to the named entity. Do NOT write or modify any code — this is read-only exploration.

Produce a single markdown artefact covering the sections below. If a section has no relevant findings, include it with "None found." Do not omit sections. Do not speculate — only document what exists in the code.

---

## Artefact Template

```markdown
---
entity: { Entity Name }
compiled_at: { ISO 8601 timestamp }
branch: { git branch name, or "unknown" if not a git repo }
commit: { short SHA, or "unknown" if not a git repo }
---

# {Entity Name}

## Purpose

What this entity represents in the domain. One or two sentences.

## Schema

| Field | Type | Constraints | Default | Notes |
| ----- | ---- | ----------- | ------- | ----- |
| ...   | ...  | ...         | ...     | ...   |

Include: primary keys, foreign keys, nullable/required, unique constraints, enums, check constraints.

## Relationships

| Relation | Target Entity | Type | Foreign Key | Notes |
| -------- | ------------- | ---- | ----------- | ----- |
| ...      | ...           | ...  | ...         | ...   |

Types: belongs_to, has_one, has_many, many_to_many (or equivalent in the codebase's framework).

## Business Rules & Invariants

- Validations applied on create/update
- State machines or lifecycle transitions
- Required conditions or preconditions
- Derived/computed fields and how they're calculated

## Behavioral Toggles

Configuration, feature flags, or environment settings that change how this entity behaves. For each, document:

- What it controls
- Default behavior (when absent or off)
- How behavior changes when active

## Key Modules & Functions

| Module/File | Responsibility | Key Functions |
| ----------- | -------------- | ------------- |
| ...         | ...            | ...           |

Focus on: where the entity is defined, where it's created/updated/deleted, where business logic lives.

## Access Patterns

### Reads

How the entity is typically queried — **grouped by surface** (e.g. API, GraphQL, web UI, CLI, internal). For each, document:

- What fields/relations are exposed
- Available filters, sorting, pagination
- Authorization or scoping rules

If all surfaces share identical read behavior, list the surfaces but document the shared behavior once.

### Writes

How the entity is created, updated, and deleted — **grouped by surface** (e.g. internal, web UI, API, CSV import, background jobs, integrations). For each surface, document:

- Which code path handles it
- How validation or required fields differ from other surfaces
- What side effects are triggered (events, recalculations, cascades)

If all surfaces share identical behavior, say so. If they diverge, the differences are the most important thing to capture.

### Indexes

Indexes that exist on the underlying table.

## Domain Context

- Which bounded context or module namespace owns this entity
- Upstream dependencies (what must exist before this entity)
- Downstream dependents (what breaks if this entity changes)
```

## Rules

1. **Be thorough** — Check schema definitions, migrations, model/module files, context modules, test files, and query modules.
2. **Be precise** — Use exact field names, types, and module paths from the code. No paraphrasing.
3. **Be self-contained** — Another LLM reading only this artefact should have enough context to write correct code against this entity.
4. **Cite locations** — Include file paths so a reader can verify.
5. **No invention** — If you can't find it in the code, don't infer it.
6. **Readable tables** — If using markdown tables, keep them human-readable: align columns, use concise cell values, and break wide tables into multiple narrower tables rather than producing a single table that requires horizontal scrolling.
