---
name: lartefact-compile
description: Use when asked to compile a full artefact, document a component end-to-end, or run the full lartefact pipeline — "compile artefact for X", "lartefact X", "document X"
---

# Compile (Meta Skill)

Orchestrates the full lartefact pipeline. Does NOT have a PROMPT.md.

## Triggers

- "compile artefact for X"
- "lartefact X"
- "document X"

## Inputs

1. **Component name** (required)
2. **Codebase path** (optional) — defaults to current working directory
3. **Output path** (optional) — where to write artefacts. If not provided, ask.

## Process

**Step 1:** Read and follow `../code/SKILL.md`. Write output to disk before proceeding.

**Step 2:** Read and follow `../code_history/SKILL.md`. Pass the artefact from Step 1 as the code artefact path input. Write output to disk before proceeding.

Do not stop after Step 1. The full compilation is both code and code_history.

**Step 3 (data):** Only runs when the user explicitly requests it or says "compile everything for X". Read and follow `../data/SKILL.md`.

> **Important:** Write each artefact to disk before starting the next step.
