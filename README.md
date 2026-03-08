# lartefact

A Claude Code skill library for compiling structured knowledge artefacts about codebase entities. Artefacts are consumed by LLMs in downstream tasks — writing queries, mutations, business logic, tests, assigning reviewers, planning refactors.

## Skills

### `component` — Entity Artefact

Explores a codebase and produces a self-contained markdown document covering everything an LLM needs to work with an entity:

- Schema (fields, types, constraints, defaults)
- Relationships (belongs_to, has_many, many_to_many, etc.)
- Business rules and invariants
- Behavioral toggles and feature flags
- Key modules and functions
- Access patterns (reads, writes, indexes)
- Domain context (ownership, upstream/downstream dependencies)

**Triggers:** "compile entity artefact for X", "create an artefact for X", "document the X entity"

**Output:** `{entity_name}.md`

---

### `history` — History Artefact

Analyzes git history for an entity's files and produces a sociotechnical report covering:

- Ownership (primary authors, recommended reviewers, knowledge concentration risk)
- Change patterns (frequency, trajectory, hotspots)
- Evolution (what changed, when, and why)
- Major changes (rewrites, refactors, feature additions)
- Risk signals (bug fix patterns, fragile areas)
- Tech debt indicators

**Triggers:** "compile history artefact for X", "who owns X?", "what's the history of X?"

**Output:** `{entity_name}_history.md`

---

## Usage

Both skills are invoked via Claude Code's `Skill` tool. Each skill gathers:

| Input | Required | Description |
| ----- | -------- | ----------- |
| Entity name | yes | e.g. `User`, `Order`, `Invoice` |
| Codebase path | yes | path to the codebase on disk |
| Output path | no | where to write the artefact |
| Entity artefact path | no (`history` only) | bootstraps file discovery from an existing entity artefact |

The skills are read-only — they never write or modify code.
