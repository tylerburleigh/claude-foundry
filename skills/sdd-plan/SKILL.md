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
- **Entry** → UnderstandIntent
  - Analyze[Explore|Glob/Grep] → LSP analysis
  - CreatePhasePlan → `plan action="create"`
  - [AI Review?] → `plan action="review"` ↻ until no blockers
  - (GATE: phase approval)
  - SchemaExport → SpecCreate → `authoring action="spec-create"`
  - Validate → `spec action="validate"` ↻ [errors?] → fix
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

## MCP Tooling

This skill operates entirely through the Foundry MCP server (`foundry-mcp`). Tools use the router+action pattern: `mcp__plugin_foundry_foundry-mcp__<router>` with `action="<action>"`.

| Router | Actions | Purpose |
|--------|---------|---------|
| **plan** | `create`, `list`, `review` | Create, list, and review markdown plans |
| **authoring** | `spec-create` | Generate specification files |
| **spec** | `validate`, `validate-fix`, `list`, `get`, `get-hierarchy`, `schema-export` | Validation and querying |

**Critical Rules:**
- **ALWAYS** use MCP tools for codebase analysis when documentation exists
- **NEVER** read spec JSON files directly with `Read()` or shell commands
- Fall back to `Glob`/`Grep`/`Read` only when documentation is unavailable

## Core Workflow

### File Path Policy (Critical)

For every node of type `task` with `metadata.task_category` of `implementation` or `refactoring`, set `metadata.file_path` to a **real repo-relative path in the target workspace** (e.g. `services/ai-guide/internal_reasoning/step_setPrompt.go`).

- Do **not** guess or use placeholders like `task-1-1.py`.
- If the correct file is unclear during planning, prefer changing the task_category to `investigation` and add a follow-up implementation task once the correct file path is known.

This keeps reviews and task execution grounded in the actual codebase.


### Step 1: Understand Intent

Before creating any plan, deeply understand what needs to be accomplished:
- **Core objective**: What is the primary goal?
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

### Step 3: Create High-Level Phase Plan

For complex features, create a markdown phase plan first and get user approval before detailed spec creation:

```bash
mcp__plugin_foundry_foundry-mcp__plan action="create" name="Feature Name" template="detailed"
```

> See `references/phase-plan-template.md` for template structure.

### Step 3.5: AI Review of Phase Plan

Before converting to JSON spec, get AI feedback to catch issues early:

```bash
mcp__plugin_foundry_foundry-mcp__plan action="review" plan_path="specs/.plans/feature-name.md" review_type="full"
```

Review types: `full`, `quick`, `security`, `feasibility`. Iterate until no critical blockers.

> See `references/ai-review.md` for review output format and iteration workflow.

### Step 4: Create JSON Specification

After phase approval, get the spec schema and create the JSON spec:
```bash
mcp__plugin_foundry_foundry-mcp__spec action="schema-export"
mcp__plugin_foundry_foundry-mcp__authoring action="spec-create" name="feature-name" template="medium"
```

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
- Phase plan template → `references/phase-plan-template.md`
- AI review workflow → `references/ai-review.md`
- JSON spec structure → `references/json-spec.md`
- Task hierarchy & dependencies → `references/task-hierarchy.md`
- Codebase analysis patterns → `references/codebase-analysis.md`
- Troubleshooting → `references/troubleshooting.md`

See **[references/](./references/)**
