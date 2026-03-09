# Lartefact Plugin Setup Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Turn the lartefact directory into a self-contained Claude Code plugin by reorganising skills into a `skills/` subdirectory and adding plugin metadata.

**Architecture:** Add a `.claude-plugin/plugin.json` metadata file (mirrors savant's structure), then move the 5 existing skill folders (`code`, `code_history`, `compile`, `data`, `visual`) into a new `skills/` subdirectory. No skill content is modified.

**Tech Stack:** File system only — no code changes.

---

### Task 1: Create the `skills/` subdirectory and move skill folders

**Files:**
- Create: `lartefact/skills/` (directory)
- Move: `lartefact/code/` → `lartefact/skills/code/`
- Move: `lartefact/code_history/` → `lartefact/skills/code_history/`
- Move: `lartefact/compile/` → `lartefact/skills/compile/`
- Move: `lartefact/data/` → `lartefact/skills/data/`
- Move: `lartefact/visual/` → `lartefact/skills/visual/`

**Step 1: Move skill folders**

```bash
cd /Users/ryanzidago/Projects/lartefact
mkdir skills
mv code code_history compile data visual skills/
```

**Step 2: Verify structure**

```bash
ls skills/
```

Expected output:
```
code    code_history    compile    data    visual
```

**Step 3: Commit**

```bash
git add -A
git commit -m "refactor: move skills into skills/ subdirectory"
```

---

### Task 2: Add plugin metadata

**Files:**
- Create: `lartefact/.claude-plugin/plugin.json`

**Step 1: Create the directory and file**

```bash
mkdir .claude-plugin
```

Then create `.claude-plugin/plugin.json` with this content:

```json
{
  "name": "lartefact",
  "version": "0.1.0",
  "description": "Compile structured knowledge artefacts about codebase components",
  "author": {
    "name": "Ryan Zidago"
  },
  "keywords": ["artefact", "documentation", "code", "history", "data", "visual"]
}
```

**Step 2: Verify file exists and is valid JSON**

```bash
cat .claude-plugin/plugin.json
```

**Step 3: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: add Claude plugin metadata"
```

---

### Task 3: Verify final structure

**Step 1: Check the full tree**

```bash
find /Users/ryanzidago/Projects/lartefact -not -path '*/.git/*' | sort
```

Expected:
```
lartefact/
├── .claude-plugin/
│   └── plugin.json
├── README.md
├── docs/
│   └── plans/
│       └── 2026-03-09-lartefact-plugin-setup.md
└── skills/
    ├── code/
    │   ├── PROMPT.md
    │   └── SKILL.md
    ├── code_history/
    │   ├── PROMPT.md
    │   └── SKILL.md
    ├── compile/
    │   └── SKILL.md
    ├── data/
    │   ├── PROMPT.md
    │   └── SKILL.md
    └── visual/
        ├── PROMPT.md
        └── SKILL.md
```
