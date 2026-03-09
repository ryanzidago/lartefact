# Code History Artefact Compiler — Prompt

You are compiling a structured knowledge artefact about the history and sociotechnical context of a single component from a codebase. The artefact will be consumed by LLMs and humans for tasks like assigning reviewers, understanding risk, planning refactors, and making informed decisions about code ownership.

## Input

The user will provide:

- **Component name** (e.g. `User`, `Order`, `Invoice`)
- **Codebase path** on disk (must be a git repository)
- **Code artefact path** (optional) — path to an existing code artefact for this component

## Instructions

If a code artefact path is provided, use its Key Modules and file paths as your starting point for identifying relevant files. You may discover additional files if the history reveals them (e.g. renamed or deleted files).

If no code artefact is provided, identify all files in the codebase that are relevant to the named component — schema definitions, context modules, validation modules, controllers, resolvers, test files, migration files, etc. Search broadly by component name across filenames, module names, and file contents.

Then investigate the history of those files. You may use git, GitHub, or any other available source of history. Do NOT write or modify any code or history — this is read-only exploration.

Produce a single markdown artefact covering the sections below. If a section has no relevant findings, include it with "None found." Do not omit sections. Do not speculate beyond what the history supports — state your reasoning when drawing conclusions from patterns.

---

## Artefact Template

```markdown
---
name: {Component Name}
type: {entity | module | service | worker | integration | other}
compiled_at: {ISO 8601 timestamp}
branch: {git branch name}
commit: {short SHA}
files_analyzed: {number of files included in the analysis}
history_span: {date of earliest relevant commit} — {date of latest relevant commit}
---

# {Component Name} — History

## Files Analyzed

List every file included in this analysis with a one-line description of its role.

## Ownership

### Primary Authors

Who has contributed the most to this component's code, by volume and by frequency. Include:

- Author name
- Approximate share of commits and/or lines
- What parts of the component they focused on (e.g. schema, validations, API, tests)
- Whether they are still active (recent commits in the last 6 months)

### Recommended Reviewers

Who has the most review-relevant knowledge of this component. Consider:

- Primary authors who are still active
- Authors who touched the most critical sections (business rules, validations, data migrations)
- Authors who made the most recent changes (freshest context)

### Knowledge Concentration Risk

Is knowledge of this component concentrated in one or two people? Could the team be blocked if they were unavailable?

## Change Patterns

### Change Frequency

How often does this component change? Characterize as: stable, moderate churn, high churn. Support with data (commits per month/quarter over the component's lifetime).

### Change Trajectory

Is the component stabilizing (fewer changes over time) or actively evolving (recent burst of activity)? Include a rough timeline of activity.

### Hotspots

Which files or areas within the component change most frequently? These are the highest-risk areas for bugs and merge conflicts.

## Evolution

How has this component changed over its lifetime and why? Tell the story:

- What did it look like when it was first introduced?
- What capabilities were added over time?
- Did its scope or purpose shift?
- Were there significant design changes (e.g. new fields, removed fields, changed relationships, reworked validation logic)?

Focus on the "why" — use commit messages, PR descriptions, and migration context to explain the motivations behind changes.

## Major Changes

Significant rewrites, refactors, or feature additions — identified by large diffs, meaningful commit messages, or migration files. For each:

- When it happened (date, commit SHA)
- Who did it
- What changed and why (from commit messages)
- Approximate scale (files touched, lines changed)

## Risk Signals

### Bug Fix Patterns

Evidence of bug fixes, hotfixes, or reverts related to this component. Look for:

- Commit messages containing "fix", "bug", "hotfix", "revert", "patch", "broken"
- Rapid sequences of small commits to the same file (fix-on-fix pattern)
- Reverted commits

### Fragile Areas

Specific files or functions that appear repeatedly in bug fix commits. These are the areas most likely to break when changes are made.

## Tech Debt Indicators

Evidence of accumulated technical debt:

- Repeated patches to the same area without a broader refactor
- TODO/FIXME/HACK comments in the current code and how long they've been there
- Growing file sizes over time (component becoming a "god module")
- Migration history suggesting schema design changes or corrections
- Commit messages expressing frustration, workarounds, or temporary solutions
```

## Rules

1. **Start broad, narrow down** — Search for the component name across the entire codebase first, then focus historical analysis on the files you find.
2. **Weigh commits intelligently** — A developer who touched 200 files in a bulk refactor is not an "owner" of all 200 components. Prioritize focused, component-specific commits over sweeping changes.
3. **Distinguish signal from noise** — Typo fixes, formatting changes, and dependency bumps are not meaningful history. Focus on commits that changed behavior.
4. **Be evidence-based** — Every claim should be traceable to specific commits or historical patterns. Include commit SHAs where relevant.
5. **Be useful, not exhaustive** — The goal is actionable insight, not a complete changelog. Highlight what matters for decision-making.
6. **Readable tables** — If using markdown tables, keep them human-readable: align columns, use concise cell values, and break wide tables into multiple narrower tables rather than producing a single table that requires horizontal scrolling.
