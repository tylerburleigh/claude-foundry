# Task Lifecycle Workflows

Starting, blocking, and completing tasks.

## Starting a Task

Mark a task as in_progress when you begin work:

```bash
mcp__plugin_foundry_foundry-mcp__task action="start" spec_id={spec-id} task_id={task-id}
```

The MCP tool automatically records the start timestamp for tracking purposes.

---

## Handling Blockers

### Mark Task as Blocked

When a task cannot proceed:

```bash
mcp__plugin_foundry_foundry-mcp__task action="block" spec_id={spec-id} task_id={task-id} reason="Description of blocker" blocker_type={type} ticket="TICKET-123"
```

**Blocker types:**
- `dependency` - Waiting on external dependency
- `technical` - Technical issue blocking progress
- `resource` - Resource unavailability
- `decision` - Awaiting architectural/product decision

### Unblock Task

When blocker is resolved:

```bash
mcp__plugin_foundry_foundry-mcp__task action="unblock" spec_id={spec-id} task_id={task-id} resolution="Description of how it was resolved"
```

### List All Blockers

```bash
mcp__plugin_foundry_foundry-mcp__task action="list-blocked" spec_id={spec-id}
```

---

## Completing Tasks

### Complete a Task (Recommended: Atomic Status + Journal)

When finishing a task, use `task action="complete"` to atomically mark it complete AND create a journal entry:

```bash
# Complete with automatic journal entry
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id} journal_entry="Successfully implemented JWT authentication with token refresh. All tests passing including edge cases for expired tokens."

# Add a completion note
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id} completion_note="All tests passing" journal_entry="Implemented authentication successfully."
```

**What `task action="complete"` does automatically:**
1. Updates task status to `completed`
2. Records completion timestamp
3. Creates a journal entry documenting the completion
4. Clears the `needs_journaling` flag
5. Syncs metadata and recalculates progress
6. Automatically journals parent nodes (phases, groups) that auto-complete

### Parent Node Journaling

When completing a task causes parent nodes (phases or task groups) to auto-complete, the tool automatically creates journal entries for those parents:

- **Automatic detection**: The system detects when all child tasks in a phase/group are completed
- **Automatic journaling**: Creates journal entries like "Phase Completed: Phase 1" for each auto-completed parent
- **No manual action needed**: You don't need to manually journal parent completions
- **Hierarchical**: Works for multiple levels (e.g., completing a task can journal both its group AND its phase)

### Alternative: Status-Only Update (Not Recommended for Completion)

If you need to mark a task completed without journaling (rare), use:

```bash
mcp__plugin_foundry_foundry-mcp__task action="update-status" spec_id={spec-id} task_id={task-id} status="completed" note="Brief completion note"
```

**Warning:** Use `task action="complete"` instead to ensure proper journaling.

### Complete a Spec

When all phases are verified and complete:

```bash
# Complete spec (updates metadata, regenerates docs, moves to completed/)
mcp__plugin_foundry_foundry-mcp__lifecycle action="complete" spec_id={spec-id}
```
