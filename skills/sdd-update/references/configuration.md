# Configuration Reference

Configuration options for sdd-update behavior.

## Fidelity Review Configuration

The optional pre-completion fidelity review behavior can be configured via `.claude/sdd_config.json`:

```json
{
  "fidelity_review": {
    "enabled": true,
    "on_task_complete": "prompt",
    "on_phase_complete": "always",
    "skip_categories": ["investigation", "research"],
    "min_task_complexity": "medium"
  }
}
```

### Configuration Options

- `enabled` (boolean, default: `true`) - Master switch for fidelity review features
- `on_task_complete` (string, default: `"prompt"`) - When to offer fidelity review for task completion:
  - `"always"` - Automatically run fidelity review before marking any task complete
  - `"prompt"` - Ask user if they want to run fidelity review (recommended)
  - `"never"` - Skip automatic prompts, only use manual invocation or verification tasks

- `on_phase_complete` (string, default: `"always"`) - When to offer fidelity review for phase completion:
  - `"always"` - Automatically run phase-level fidelity review when all tasks in phase complete
  - `"prompt"` - Ask user if they want to run phase review
  - `"never"` - Skip automatic phase reviews

- `skip_categories` (array, default: `[]`) - Task categories that don't require fidelity review:
  - Common values: `["investigation", "research", "decision"]`
  - Tasks with these categories will skip automatic review prompts

- `min_task_complexity` (string, default: `"low"`) - Minimum task complexity for automatic review:
  - `"low"` - Review all tasks (most thorough)
  - `"medium"` - Only review medium/high complexity tasks
  - `"high"` - Only review high complexity tasks (least intrusive)

### When Fidelity Review is Triggered

Based on configuration, when completing a task via `sdd-update`, the system will:

1. Check if task category is in `skip_categories` - skip if true
2. Check task complexity against `min_task_complexity` - skip if below threshold
3. Check `on_task_complete` setting:
   - `"always"` - Automatically invoke fidelity review via `Skill(foundry:sdd-fidelity-review)`
   - `"prompt"` - Ask user: "Run fidelity review before completing?"
   - `"never"` - Skip (user can still manually invoke)

**Note:** Verification tasks with `verification_type: "fidelity"` always run regardless of configuration.
