---
name: sdd-modify
description: Apply spec modifications systematically. Use to apply review feedback, bulk modifications, or interactive spec updates with safety checks, validation, and rollback support.
---

# Spec-Driven Development: Modify Skill

## When to Use This Skill

Use `Skill(foundry:sdd-modify)` to:
- **Apply review feedback** from sdd-plan-review or sdd-fidelity-review
- **Bulk modify** task descriptions, metadata, or verification steps
- **Add/remove tasks** or verification nodes
- **Update task metadata** across multiple nodes
- **Interactive spec updates** with preview and validation

**Do NOT use for:**
- Status tracking or task completion (use `sdd-update`)
- Creating specifications (use `sdd-plan`)
- Finding next actionable task (use `sdd-next`)
- Reading spec files directly (use MCP tools)

## Core Philosophy

**Safe Systematic Changes**: Modifications are applied with automatic backup, preview (dry-run), validation, and rollback capabilities. Never apply changes without previewing first. The spec file is the source of truth, and all changes flow through MCP tools that handle validation automatically.

## MCP Tooling

| Router | Key Actions |
|--------|-------------|
| `spec` | `apply-plan`, `validate`, `validate-fix` |
| `review` | `parse-feedback` |
| `authoring` | `task-add`, `task-remove` |
| `task` | `update-metadata` |

**Critical Rules:**
- **ALWAYS** use MCP tools for spec operations
- **NEVER** use `Read()` on spec JSON files
- **NEVER** use shell commands (`cat`, `grep`, `jq`) on specs
- **ALWAYS** preview with dry-run before applying modifications

## Skill Family

This skill is part of the **Spec-Driven Development** workflow:

```
sdd-plan --> sdd-plan-review --> sdd-modify (this skill) --> sdd-next --> Implementation --> sdd-update
                                       ^
             sdd-fidelity-review ------+
```

Use after `sdd-plan-review` or `sdd-fidelity-review` to apply suggested changes.

**Key Distinction:** sdd-modify = structural changes; sdd-update = status updates.

> For detailed comparison, see `reference.md#comparison`

## Core Workflow

1. **Understand** → Identify spec ID, source (review report or modifications JSON), mode
2. **Parse** → Convert review feedback to modifications JSON (if needed)
3. **Preview** → Always dry-run first
4. **Apply** → Apply modifications (auto-backup, validation, rollback on failure)
5. **Validate** → Confirm spec is valid

**Essential Command:**

```bash
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="{spec-id}" modifications_file="{path}" dry_run=true
```

> For detailed workflow steps with examples, see `reference.md#workflow-details`

## Supported Operations

Operations: `update_task`, `add_verification`, `update_metadata`, `batch_update`, `add_node`, `remove_node`

> For operation formats and examples, see `reference.md#supported-operations`

## Safety Features

- **Automatic backup** before every modification (`specs/.backups/{spec-id}-{timestamp}.json`)
- **Dry-run preview** before applying any changes
- **Validation** after every apply
- **Rollback** if validation fails
- **Idempotent** operations (safe to retry)

> For rollback and recovery procedures, see `reference.md#error-handling`

## Output Format

Return a structured markdown summary including: spec ID, source, changes applied, backup location, validation status, and next steps.

> For full template, see `reference.md#output-format`

## Detailed Reference

For comprehensive documentation including:
- Detailed workflow steps with examples
- Operation formats and JSON structures
- Error handling and rollback procedures
- Direct modification patterns (when to skip review parsing)
- Troubleshooting guide

See **[reference.md](./reference.md)**
