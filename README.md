# lartefact

A Claude Code skill library for compiling structured knowledge artefacts about codebase components. Artefacts are consumed by LLMs in downstream tasks — writing queries, mutations, business logic, tests, assigning reviewers, planning refactors.

Two sources of truth about a system: what the **code** says should happen, and what the **data** says actually happened. `code_history` is the temporal layer on `code`.

## Artefacts

### `compile` — Full Pipeline (Meta Skill)

Orchestrates the full lartefact pipeline. Runs `code` then `code_history` in sequence, optionally followed by `data`.

**Triggers:** "compile artefact for X", "lartefact X", "document X"

**Output:** runs `code` and `code_history` (and optionally `data`) — see each skill for output files

---

### `code` — Code Artefact

Explores a codebase and produces a self-contained markdown document covering everything an LLM needs to work with a component:

- Schema (fields, types, constraints, defaults)
- Relationships (belongs_to, has_many, many_to_many, etc.)
- Business rules and invariants
- Behavioral toggles and feature flags
- Key modules and functions
- Access patterns (reads, writes, indexes)
- Domain context (ownership, upstream/downstream dependencies)

**Triggers:** "compile code artefact for X", "document X code", "document the X component"

**Output:** `{component_name}.md`

---

### `code_history` — Code History Artefact

Analyzes git history for a component's files and produces a sociotechnical report covering:

- Ownership (primary authors, recommended reviewers, knowledge concentration risk)
- Change patterns (frequency, trajectory, hotspots)
- Evolution (what changed, when, and why)
- Major changes (rewrites, refactors, feature additions)
- Risk signals (bug fix patterns, fragile areas)
- Tech debt indicators

**Triggers:** "compile history artefact for X", "who owns X?", "what's the history of X?"

**Output:** `{component_name}_history.md`

---

### `data` — Data Artefact

Queries a live database and produces a factual profile of a component's real-world data:

- Table shape (row count, growth rate, soft-delete ratio)
- Column profiles (distributions, null rates, cardinality)
- Relationship cardinality (actual vs theoretical)
- Index utilisation
- Access statistics
- Anomalies

**Triggers:** "compile data profile for X", "profile the X table", "what does X data look like?"

**Output:** `{component_name}_data_profile.md`

---

### `visual` — Visual Surface Map

Explores a frontend codebase to find every place a domain concept surfaces in the UI. Produces a high-level map of where it appears, what it enables, and who it's for.

- Routes and pages where the concept appears
- Who can see it (roles, permissions, feature flags)
- What users can do (view, edit, create, approve, export, etc.)
- Why each surface exists

**Triggers:** "where does X appear in the UI?", "map the visual surfaces for X", "how is X exposed visually?"

**Output:** inline — reported directly in the conversation, not written to disk

---

## Workflow

`code` always runs first. `code_history` and `data` both bootstrap from it. `visual` is independent.

```
Step 1 — code           (always first)
Step 2 — code_history   (uses Step 1 output)
Step 3 — data           (uses Step 1 output, requires DB access)

Step 4 — visual         (independent, no persisted output)
```

Use `compile` to run Steps 1–2 automatically (and optionally Step 3).

## Usage

All skills are invoked via Claude Code's `Skill` tool. Each skill gathers:

| Input | Required | Description |
| ----- | -------- | ----------- |
| Component name | yes | e.g. `User`, `Order`, `Invoice` |
| Codebase path | yes | path to the codebase on disk |
| Output path | no | where to write the artefact |
| Code artefact path | no (`code_history`, `data`, `visual` only) | bootstraps from an existing code artefact |

The skills are read-only — they never write or modify code or data.
