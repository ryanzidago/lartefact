# Code Artefact Compiler — Prompt

You are compiling a structured knowledge artefact about a single component from a codebase. The component may be an entity (database-backed record), a module, a class, a service, or any other meaningful unit of code. The artefact will be consumed by LLMs and humans — so it must be accurate, complete, and self-contained.

## Input

The user will provide:

- **Component name** (e.g. `User`, `HolidayCalculator`, `PaymentService`)
- **Codebase path** on disk

## Instructions

Explore the codebase to find everything relevant to the named component. Do NOT write or modify any code — this is read-only exploration.

Determine what kind of component this is based on what you find in the code, then produce a single markdown artefact using the template below. Include every **Core** section. Include **Data** sections only if the component is backed by a database table. Include **Logic** sections only if the component encapsulates meaningful business logic, algorithms, or a public interface consumed by other modules.

If a section has no relevant findings, include it with "None found." Do not omit applicable sections. Do not speculate — only document what exists in the code.

---

## Artefact Template

```markdown
---
name: { Component Name }
type: { entity | module | service | worker | integration | other }
compiled_at: { ISO 8601 timestamp }
branch: { git branch name, or "unknown" if not a git repo }
commit: { short SHA, or "unknown" if not a git repo }
---

# {Component Name}

## Purpose

What this component does and why it exists. One or two sentences.

# ── Core (always include) ────────────────────────────────────────────

## Business Rules & Invariants

- Validations, constraints, preconditions
- State machines or lifecycle transitions
- Derived/computed values and how they're calculated
- Edge cases and special handling

## Behavioral Toggles

Configuration, feature flags, or environment settings that change how this component behaves. For each, document:

- What it controls
- Default behavior (when absent or off)
- How behavior changes when active

## Key Modules & Functions

| Module/File | Responsibility | Key Functions |
| ----------- | -------------- | ------------- |
| ...         | ...            | ...           |

Focus on: where the component is defined, where its logic lives, where it's called from.

## Domain Context

- Which bounded context or module namespace owns this component
- Upstream dependencies (what must exist or be available)
- Downstream dependents (what breaks if this component changes)

# ── Data (include if database-backed) ────────────────────────────────

## Schema

| Field | Type | Constraints | Default | Notes |
| ----- | ---- | ----------- | ------- | ----- |
| ...   | ...  | ...         | ...     | ...   |

Include: primary keys, foreign keys, nullable/required, unique constraints, enums, check constraints.

## Relationships

| Relation | Target Entity | Type | Foreign Key | Notes |
| -------- | ------------- | ---- | ----------- | ----- |
| ...      | ...           | ...  | ...         | ...   |

## Access Patterns

### Reads

How the component is typically queried — **grouped by surface** (e.g. API, GraphQL, web UI, CLI, internal). For each, document:

- What fields/relations are exposed
- Available filters, sorting, pagination
- Authorization or scoping rules

If all surfaces share identical read behavior, list the surfaces but document the shared behavior once.

### Writes

How the component is created, updated, and deleted — **grouped by surface** (e.g. internal, web UI, API, CSV import, background jobs, integrations). For each surface, document:

- Which code path handles it
- How validation or required fields differ from other surfaces
- What side effects are triggered (events, recalculations, cascades)

If all surfaces share identical behavior, say so. If they diverge, the differences are the most important thing to capture.

### Indexes

Indexes that exist on the underlying table.

# ── Logic (include if meaningful business logic or public interface) ──

## Interface

Public functions this component exposes — the contract other code depends on. For each:

- Function signature (name, inputs, outputs)
- What it does
- Key assumptions or preconditions

## Dependencies

What this component reads from or calls:

- Entities or tables it queries
- Services or modules it delegates to
- External APIs or integrations
- Configuration it reads

## Consumers

Who calls this component and why:

- Other modules, services, or contexts that depend on it
- How they use it (which functions, in what workflows)
- Whether it's called synchronously or asynchronously
```

## Rules

1. **Be thorough** — Check schema definitions, migrations, model/module files, context modules, test files, and query modules.
2. **Be precise** — Use exact field names, types, and module paths from the code. No paraphrasing.
3. **Be self-contained** — Another LLM reading only this artefact should have enough context to write correct code involving this component.
4. **Cite locations** — Include file paths so a reader can verify.
5. **No invention** — If you can't find it in the code, don't infer it.
6. **Readable tables** — If using markdown tables, keep them human-readable: align columns, use concise cell values, and break wide tables into multiple narrower tables rather than producing a single table that requires horizontal scrolling.
7. **Adapt to the component** — Not every section applies to every component. An entity without business logic doesn't need Interface or Consumers. A pure logic module doesn't need Schema or Indexes. Use judgment.
