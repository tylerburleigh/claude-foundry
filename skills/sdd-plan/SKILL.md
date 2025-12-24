---
name: sdd-plan
description: Plan-first development methodology that creates detailed specifications before coding. Use when building features, refactoring code, or implementing complex changes. Creates structured plans with phases, file-level details, and verification steps to prevent drift and ensure production-ready code.
---

# Spec-Driven Development: Planning Skill

## Overview

`Skill(foundry:sdd-plan)` creates detailed JSON specifications before any code is written. It analyzes the codebase, designs a phased implementation approach, and produces structured task hierarchies with verification steps. The skill enforces staged planning for complex features - first creating a high-level phase plan for user approval, then expanding into detailed tasks.

**Core capabilities:**
- Analyze codebase structure using documentation queries
- Design phased implementation approaches
- Create JSON specifications with task hierarchies
- Define verification steps for each phase
- Validate specifications automatically after creation

Use this skill when building new features, performing complex refactoring, or implementing changes that span multiple files.

### Flow

> `[x?]`=decision · `(GATE)`=user approval · `→`=sequence · `↻`=loop · `§`=section ref

```
- **Entry** → UnderstandIntent → Analyze[Explore|LSP]
  - `authoring action="spec-create"` → scaffold spec
  - `authoring action="phase-add-bulk"` ↻ per phase
  - [Fine-tune?] → `Skill(sdd-modify)` for task edits
  - `authoring action="spec-update-frontmatter"` → mission/metadata
  - `spec action="validate"` ↻ [errors?] → fix
  - **Exit** → specs/pending/{spec-id}.json
```

## Skill Family

This skill is part of the **Spec-Driven Development** workflow:

```
sdd-plan (this skill) --> sdd-plan-review --> sdd-modify --> sdd-next --> Implementation --> sdd-update
```

After creating a spec, use `Skill(foundry:sdd-plan-review)` for AI-powered review or proceed directly to `Skill(foundry:sdd-next)` for task execution.

## When to Use This Skill

Use `Skill(foundry:sdd-plan)` for:
- New features or significant functionality additions
- Complex refactoring across multiple files
- API integrations or external service connections
- Architecture changes or system redesigns
- Any task requiring precision and reliability
- Large codebases where context drift is a risk

**Do NOT use for:**
- Simple one-file changes or bug fixes
- Trivial modifications or formatting changes
- Exploratory prototyping or spikes
- Quick experiments or proof-of-concepts
- Updating existing specs (use `Skill(foundry:sdd-update)`)
- Finding next task (use `Skill(foundry:sdd-next)`)

## Phase-First Authoring Workflow

For efficient spec creation, use this phase-first approach that minimizes manual JSON editing:

```
┌─────────────────────────────────────────────────────────────────────┐
│  1. Create Spec      2. Add Phases       3. Fine-Tune    4. Update  │
│  ───────────────     ─────────────       ──────────      Metadata   │
│  spec-create     →   phase-add-bulk  →   sdd-modify  →   frontmatter│
│  (scaffold)          (atomic macro)      (task edits)    (mission)  │
└─────────────────────────────────────────────────────────────────────┘
```

**Step 1: Create spec from template**
```bash
mcp__plugin_foundry_foundry-mcp__authoring action="spec-create" name="my-feature" template="medium"
```

**Step 2: Add phases with tasks using the bulk macro**
```bash
mcp__plugin_foundry_foundry-mcp__authoring action="phase-add-bulk" spec_id="{spec-id}" phase='{"title": "Implementation", "description": "Core feature work"}' tasks='[{"type": "task", "title": "Build core logic", "estimated_hours": 4}, {"type": "verify", "title": "Run tests", "verification_type": "run-tests"}]'
```

**Step 3: Fine-tune tasks**
Use `Skill(foundry:sdd-modify)` or the task router to adjust individual tasks:
- Add/remove dependencies
- Update descriptions and estimates
- Reorder tasks within phases

**Step 4: Update spec metadata (frontmatter)**

After adding phases, update the spec's top-level metadata fields:
```bash
mcp__plugin_foundry_foundry-mcp__authoring action="spec-update-frontmatter" spec_id="{spec-id}" key="mission" value="Single-sentence spec objective"
```

Common metadata fields to set:
- `mission` - Single-sentence objective (required for reviews)
- `description` - Detailed description
- `version` - Spec version string
- `author` - Spec author

This workflow keeps you in MCP tooling throughout, avoiding direct JSON manipulation.

## MCP Tooling

This skill operates entirely through the Foundry MCP server (`foundry-mcp`). Tools use the router+action pattern: `mcp__plugin_foundry_foundry-mcp__<router>` with `action="<action>"`.

| Router | Actions | Purpose |
|--------|---------|---------|
| **plan** | `create`, `list`, `review` | Create, list, and review markdown plans |
| **authoring** | `spec-create`, `spec-update-frontmatter`, `phase-template`, `phase-add-bulk` | Generate specifications, update metadata, apply phase templates, add phases with tasks atomically |
| **spec** | `validate`, `validate-fix`, `list`, `find`, `stats`, `analyze` | Validation, listing, and analysis |
| **task** | `hierarchy`, `query`, `info`, `prepare` | Query hierarchy, task details, and next task context |

**Critical Rules:**
- **NEVER** read spec JSON files directly with `Read()` or shell commands

## Core Workflow

### File Path Policy (Critical)

For every node of type `task` with `metadata.task_category` of `implementation` or `refactoring`, set `metadata.file_path` to a **real repo-relative path in the target workspace** (e.g. `services/ai-guide/internal_reasoning/step_setPrompt.go`).

- Do **not** guess or use placeholders like `task-1-1.py`.
- If the correct file is unclear during planning, prefer changing the task_category to `investigation` and add a follow-up implementation task once the correct file path is known.

This keeps reviews and task execution grounded in the actual codebase.


### Step 1: Understand Intent

Before creating any plan, deeply understand what needs to be accomplished:
- **Core objective**: What is the primary goal?
- **Spec mission**: Write the single-sentence mission that will populate `metadata.mission` in the JSON spec.
- **Success criteria**: What defines "done"?
- **Constraints**: What limitations or requirements exist?

### Step 2: Analyze Codebase

Use **Explore subagents** for large codebases (prevents context bloat), or `Glob`/`Grep`/`Read` for targeted lookups.

> See `references/investigation.md` for subagent patterns.

**LSP-Enhanced Analysis:** For refactoring and change planning, use LSP tools:
- `documentSymbol` - Understand file structure before modifying
- `findReferences` - Assess impact (count affected files, map dependencies)
- `goToDefinition` - Navigate to implementations
- Zero references = safe to remove (dead code)

If LSP unavailable, fall back to Explore agents or `Grep` for symbol search.

> See `references/codebase-analysis.md` for detailed LSP patterns.

### Step 3: Create Phase Plan

For complex features, create a markdown phase plan first and get user approval before detailed spec creation:

```bash
mcp__plugin_foundry_foundry-mcp__plan action="create" name="Feature Name" template="detailed"
```

**Authoring options:**
- **Phase templates** - Pre-configured phases (`planning`, `implementation`, `testing`, `security`, `documentation`)
- **Phase-add-bulk** - Custom phases with full control over task structure
- **AI review** - Get feedback on phase plans before converting to JSON

> See `references/phase-authoring.md` for detailed usage of templates, bulk macros, and AI review.
> See `references/phase-plan-template.md` for template structure.

### Step 4: Create JSON Specification

After phase approval, create the JSON spec:
```bash
mcp__plugin_foundry_foundry-mcp__authoring action="spec-create" name="feature-name" template="medium"
```

Immediately populate the new `metadata.mission` field (and description/objectives) by summarizing the approved phase plan’s objective; this keeps downstream reviewers aligned on the spec’s why.

The spec file is created at `specs/pending/{spec-id}.json`.

> For JSON specification structure details, see `references/json-spec.md`

### Step 5: Validate the Specification

Use `Skill(foundry:sdd-validate)` to validate and auto-fix the specification:
```bash
mcp__plugin_foundry_foundry-mcp__spec action="validate" spec_id="{spec-id}"
mcp__plugin_foundry_foundry-mcp__spec action="validate-fix" spec_id="{spec-id}" auto_fix=true
```

## Size Guidelines

**Splitting recommendation:** If >6 phases or >50 tasks, recommend splitting into multiple specs.

> For size guidelines, task hierarchy, categories, and dependency tracking, see `references/task-hierarchy.md`

## Output Artifacts

1. **JSON spec file** at `specs/pending/{spec-id}.json`
2. **Validation passed** with no errors
3. **User approval** of phase structure before detailed planning

The spec file contains the full task hierarchy including phases, tasks, subtasks, and verification steps.

## Detailed Reference

- Investigation strategies → `references/investigation.md`
- Phase authoring (templates, bulk, AI review) → `references/phase-authoring.md`
- Phase plan template → `references/phase-plan-template.md`
- AI review workflow → `references/ai-review.md`
- JSON spec structure → `references/json-spec.md`
- Task hierarchy & dependencies → `references/task-hierarchy.md`
- Codebase analysis patterns → `references/codebase-analysis.md`
- Troubleshooting → `references/troubleshooting.md`
