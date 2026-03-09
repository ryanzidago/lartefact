# Visual Surface Map — Prompt

You are exploring a frontend codebase to find every place a domain concept surfaces in the UI. Your goal is to produce a high-level map of where it appears, what it enables, and who it's for — not how it's implemented.

## Input

The user will provide:

- **Domain concept** — an entity, feature, or domain area (e.g. `Invoice`, `User`, `Notification`)
- **Codebase path** on disk
- **Entity artefact path** (optional) — used to bootstrap discovery from known modules and files

## Instructions

Do NOT write or modify any code — this is read-only exploration.

### 1. Discover All Surfaces

Search broadly across the codebase for every place this domain concept appears in a visual context. Look in:

- Routes and navigation configurations
- Page/screen/view components
- Shared components and widgets
- Dashboard panels and summary cards
- Modal dialogs and drawers
- Notification and alert templates
- Email templates and PDF renderers (if they produce something a user sees)

Use the entity artefact (if provided) to identify module names, context functions, and schema references — then trace those into the visual layer.

Cast a wide net. The value of this skill is finding surfaces you didn't know existed.

### 2. For Each Surface, Report

**Where** — The page, screen, or context where it appears. Include the route/path and the file, but do not walk into child components.

**Who** — What kind of user sees this (e.g. employee, manager, admin, public). Note any permissions or feature flags that gate access.

**What** — What the surface enables at a capability level. Use verbs:

- View, edit, create, delete
- Approve, reject, override
- Export, print, share
- Filter, search, sort
- Summarise, compare, aggregate

Do NOT describe form fields, component props, UI controls, or implementation details. "User can view and export their past invoices" is correct. "A table component with sortable columns and a download button" is too detailed.

**Why** — What user need or workflow this surface serves. One sentence.

### 3. Report Structure

```
# {Domain Concept} — Visual Surfaces

## Summary

{How many surfaces found. One-line characterisation of how this concept is
exposed — e.g. "Primarily self-service with read-only visibility
for admins on a separate dashboard."}

## Surfaces

### {Surface Name}

- **Route**: {route or path}
- **File**: {primary file path}
- **Who**: {user role / permission}
- **Capabilities**: {verb list}
- **Purpose**: {one sentence — what user need does this serve}

### {Surface Name}

...

## Observations

{Optional. Note anything structurally interesting:
- Surfaces that seem redundant or overlapping
- Domain capabilities that exist in code but have no visual surface
- Permissions inconsistencies across surfaces
- Feature flags that gate specific surfaces}
```

## Rules

1. **Breadth over depth** — Find every surface before describing any of them. The primary failure mode is missing surfaces, not under-describing them.
2. **Capabilities, not controls** — Describe what users can do, not how the UI is built. No component names, no props, no form fields.
3. **One level deep** — Identify the page or screen. Do not walk into its component tree. The entry point file is enough.
4. **Include gated surfaces** — Surfaces behind feature flags or permissions still exist. Report them with their gate condition.
5. **Be framework-agnostic** — This skill works with React, React Native, Vue, Svelte, Flutter, SwiftUI, Jetpack Compose, LiveView, or any component-based UI.
6. **Cite file paths** — Every surface should include the file where it lives.
7. **No implementation details** — If you find yourself describing changesets, hooks, stores, or data flow, you've gone too deep. Pull back.
