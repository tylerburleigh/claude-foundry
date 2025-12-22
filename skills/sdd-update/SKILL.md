---
name: sdd-update
description: Progress tracking for spec-driven development. Use to update task status, track progress, journal decisions, move specs between folders, and maintain spec files. Handles the administrative/clerical aspects of specification documents during development.
---

# SDD Update Skill

## When to Use

- Complete tasks (atomically marks complete AND journals)
- Mark tasks as in_progress or blocked
- Document decisions/deviations via journal entries
- Move specs between lifecycle folders
- Update spec metadata

**Do NOT use for:**
- Creating specs → use `sdd-plan`
- Finding next task → use `sdd-next`
- Structural changes → use `sdd-modify`
- Writing code or running tests

> For comparison with sdd-modify, see `reference.md#comparison`

## MCP Tooling

| Router | Key Actions |
|--------|-------------|
| `task` | `complete`, `start`, `block`, `unblock`, `query` |
| `journal` | `add`, `list` |
| `lifecycle` | `activate`, `move`, `complete` |

**Critical Rules:**
- ALWAYS use MCP tools for spec operations
- NEVER use `Read()` on spec JSON files
- NEVER shell out to `cat`/`grep`/`jq` for specs

## Essential Command

```bash
# Complete a task (recommended - atomic status + journal)
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id} journal_entry="..."
```

> For all workflow commands, see `reference.md#workflow-details`

## Core Workflow

1. **Start** → `task action="start"`
2. **Progress** → `journal action="add"` for decisions/deviations
3. **Complete** → `task action="complete"` with journal_entry
4. **Move** → `lifecycle action="activate|move|complete"`

## Key Principles

1. **Update immediately** - Don't batch updates
2. **Be specific** - Vague notes aren't helpful later
3. **Document WHY** - Rationale, not just what changed
4. **Use complete** - Ensures proper journaling

## Detailed Reference

- Task states → `reference.md#task-states`
- Folder structure → `reference.md#folder-structure`
- sdd-update vs sdd-modify → `reference.md#comparison`
- Git integration → `reference.md#workflow-7-git-commit-integration`
- Troubleshooting → `reference.md#troubleshooting`
- Command reference → `reference.md#command-reference`

See **[reference.md](./reference.md)**
