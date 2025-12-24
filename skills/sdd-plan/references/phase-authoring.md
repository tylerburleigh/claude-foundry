# Phase Authoring Reference

This reference covers advanced phase authoring methods for spec creation.

## Phase Templates

Use phase templates to quickly add pre-configured phases with standard task structures. Templates include built-in verification tasks.

**Available templates:** `planning`, `implementation`, `testing`, `security`, `documentation`

```bash
# List available templates
mcp__plugin_foundry_foundry-mcp__authoring action="phase-template" template_action="list"

# Show template structure before applying
mcp__plugin_foundry_foundry-mcp__authoring action="phase-template" template_action="show" template_name="implementation"

# Apply template to an existing spec
mcp__plugin_foundry_foundry-mcp__authoring action="phase-template" template_action="apply" template_name="implementation" spec_id="{spec-id}"
```

**Template capabilities:**
- `template_action="list"` - View all available phase templates
- `template_action="show"` - Preview a template's structure (tasks, verification steps)
- `template_action="apply"` - Add template as new phase to existing spec

**When to use templates vs manual planning:**
- **Use templates** for standard phases (testing, security review, documentation)
- **Use manual planning** for custom feature-specific phases

> Templates automatically include verification tasks and follow consistent naming patterns.

## Phase-Add-Bulk Macro

For maximum control over phase structure, use `phase-add-bulk` to create a phase with all its tasks in a single atomic operation. This macro accepts a nested payload with phase metadata and task definitions.

```bash
mcp__plugin_foundry_foundry-mcp__authoring action="phase-add-bulk" spec_id="{spec-id}" phase='{"title": "Implementation", "description": "Core feature implementation", "estimated_hours": 8}' tasks='[{"type": "task", "title": "Implement core logic", "description": "Build the main functionality", "estimated_hours": 4}, {"type": "task", "title": "Add error handling", "description": "Handle edge cases and errors", "estimated_hours": 2}, {"type": "verify", "title": "Run tests", "verification_type": "run-tests"}, {"type": "verify", "title": "Fidelity review", "verification_type": "fidelity"}]'
```

**Payload structure:**

| Field | Required | Description |
|-------|----------|-------------|
| `spec_id` | Yes | Target specification ID |
| `phase.title` | Yes | Phase title |
| `phase.description` | No | Phase description |
| `phase.purpose` | No | Purpose/goal metadata |
| `phase.estimated_hours` | No | Time estimate for phase |
| `tasks` | Yes | Array of task definitions (min 1) |
| `tasks[].type` | Yes | `"task"` or `"verify"` |
| `tasks[].title` | Yes | Task title |
| `tasks[].description` | No | Task description |
| `tasks[].file_path` | No | Associated file path |
| `tasks[].estimated_hours` | No | Time estimate |
| `tasks[].verification_type` | No | For verify tasks: `"run-tests"` or `"fidelity"` |
| `position` | No | Zero-based insertion index |
| `link_previous` | No | Link to previous phase (default: true) |
| `dry_run` | No | Preview without saving (default: false) |

**When to use phase-add-bulk vs templates:**
- **Use `phase-add-bulk`** when you need custom task structures or specific verification configurations
- **Use `phase-template`** for standard phases with predefined task patterns
- **Use `phase-add-bulk`** to programmatically construct phases from plan sections

> The macro creates all nodes atomically - either the entire phase succeeds or nothing is created.

## AI Review of Phase Plans

Before converting to JSON spec, get AI feedback to catch issues early:

```bash
mcp__plugin_foundry_foundry-mcp__plan action="review" plan_path="specs/.plans/feature-name.md" review_type="full"
```

Review types: `full`, `quick`, `security`, `feasibility`. Iterate until no critical blockers.

> See `ai-review.md` for review output format and iteration workflow.
