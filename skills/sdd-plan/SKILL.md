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

This skill operates entirely through the Foundry MCP server (`foundry-mcp`). Tool names follow the `mcp__foundry-mcp__<tool-name>` pattern.

| Tool Category | MCP Tools | Purpose |
|---------------|-----------|---------|
| **Codebase Analysis** | `doc-stats`, `code-find-class`, `code-find-function`, `code-trace-calls`, `code-impact-analysis` | Understand existing code structure |
| **Plan Review** | `plan-review` | AI-powered review of markdown plans before spec creation |
| **Spec Creation** | `spec-schema-export`, `spec-create` | Generate specification files |
| **Spec Validation** | `spec-validate`, `spec-validate-fix` | Ensure spec integrity |
| **Spec Query** | `spec-list`, `spec-get`, `spec-get-hierarchy` | Inspect existing specs |

**Critical Rules:**
- **ALWAYS** use MCP tools for codebase analysis when documentation exists
- **NEVER** read spec JSON files directly with `Read()` or shell commands
- Fall back to `Glob`/`Grep`/`Read` only when documentation is unavailable

## Core Workflow

### Step 1: Understand Intent

Before creating any plan, deeply understand what needs to be accomplished:
- **Core objective**: What is the primary goal?
- **Success criteria**: What defines "done"?
- **Constraints**: What limitations or requirements exist?

### Step 2: Analyze Codebase

Check if documentation exists:
```bash
mcp__foundry-mcp__doc-stats
```

If documentation available, use for analysis:
- `mcp__foundry-mcp__code-find-class` - Find existing class implementations
- `mcp__foundry-mcp__code-find-function` - Find function implementations
- `mcp__foundry-mcp__code-trace-calls` - Trace call graphs
- `mcp__foundry-mcp__code-impact-analysis` - Analyze change impact

If documentation unavailable, fall back to `Glob`, `Grep`, and `Read`.

### 2.1 Subagent Guidance (Codebase Exploration)

For large or unfamiliar codebases, use Claude Code's built-in subagents for efficient parallel exploration:

| Scenario | Subagent | Thoroughness |
|----------|----------|--------------|
| Initial file structure discovery | Explore | quick |
| Understanding existing patterns | Explore | medium |
| Complex dependency analysis | Explore | very thorough |
| Multi-area architectural research | general-purpose | N/A |

**Example: Parallel Exploration**
```
Use the Explore agent (medium thoroughness) to find:
- All files matching the feature area pattern
- Existing implementations of similar functionality
- Test coverage for affected modules
- Related configuration files
```

**Benefits:**
- Prevents context bloat during analysis phase
- Haiku model is faster for search operations
- Keeps main context available for spec creation
- Can run multiple Explore agents in parallel for different areas

> For detailed exploration patterns, see `reference.md#parallel-investigation-strategies`

### Step 3: Create High-Level Phase Plan

For complex features (3+ phases expected), first create a phase-only markdown plan and **present to the user for approval before detailed planning**.

> For phase plan template, see `reference.md#phase-plan-template`

### Step 3.5: AI Review of Phase Plan (Optional)

Before converting your markdown plan to a formal JSON spec, you can get AI-powered feedback to catch issues early:

```bash
mcp__foundry-mcp__plan-review plan_path="./PLAN.md" review_type="full"
```

**Review Types:**
| Type | Focus |
|------|-------|
| `full` | Comprehensive 6-dimension review (completeness, architecture, sequencing, feasibility, risk, clarity) |
| `quick` | Critical blockers and questions only |
| `security` | Security-focused analysis |
| `feasibility` | Technical complexity and risk assessment |

**Review → Revise → Repeat:**
1. Create markdown plan
2. Run AI review
3. Review feedback in `./tmp/<plan-name>-review.md`
4. Revise plan based on feedback
5. Repeat until no critical blockers

**When to use AI review:**
- Complex features with 3+ phases
- Unfamiliar domains or technology
- High-risk architectural changes
- When user requests additional validation

**When to skip:**
- Simple features with clear scope
- Well-understood domain/patterns
- Time-sensitive changes
- User explicitly approves plan without review

> For review output format and iteration examples, see `reference.md#ai-plan-review`

### Step 4: Create JSON Specification

After phase approval, get the spec schema and create the JSON spec:
```bash
mcp__foundry-mcp__spec-schema-export
mcp__foundry-mcp__spec-create name="feature-name" template="medium"
```

The spec file is created at `specs/pending/{spec-id}.json`.

> For JSON specification structure details, see `reference.md#json-specification-structure`

### Step 5: Validate the Specification

Use `Skill(foundry:sdd-validate)` to validate and auto-fix the specification:
```bash
mcp__foundry-mcp__spec-validate spec_id="{spec-id}"
mcp__foundry-mcp__spec-validate-fix spec_id="{spec-id}" auto_fix=true
```

## Size Guidelines

| Complexity | Phases | Tasks | Verification Coverage |
|------------|--------|-------|----------------------|
| Short (<5 files) | 1-2 | 3-8 | 20% minimum |
| Medium (5-15 files) | 2-4 | 10-25 | 30-40% |
| Large (>15 files) | 4-6 | 25-50 | 40-50% |

**Splitting recommendation:** If >6 phases or >50 tasks, recommend splitting into multiple specs.

> For task hierarchy details, categories, and dependency tracking, see `reference.md#task-hierarchy`

## Output Artifacts

1. **JSON spec file** at `specs/pending/{spec-id}.json`
2. **Validation passed** with no errors
3. **User approval** of phase structure before detailed planning

The spec file contains the full task hierarchy including phases, tasks, subtasks, and verification steps.

## Detailed Reference

For comprehensive documentation including:
- Phase plan template with full structure
- Task hierarchy (spec, phase, group, task, subtask, verify)
- Task categories and verification types
- Dependency tracking (hard/soft deps, blocks)
- JSON specification structure
- Codebase analysis patterns
- Troubleshooting and edge cases

See **[reference.md](./reference.md)**
