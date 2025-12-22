# Post-Implementation Checklist

Before marking a task complete, verify these items.

## Required Checks

- [ ] Implementation matches task instructions
- [ ] All acceptance criteria met
- [ ] Tests written and passing
- [ ] No regressions introduced
- [ ] Code follows project patterns

## Journal Entry Requirements

Every task completion MUST include a journal entry with:

1. **What was accomplished** - Brief summary of implementation
2. **Tests run and results** - Which tests, pass/fail status
3. **Verification performed** - How you verified correctness
4. **Deviations from plan** - Any changes from original instructions
5. **Files created/modified** - List of affected files

## Example Completion

```bash
Skill(foundry:sdd-update) "Complete task task-2-3 in spec my-spec-001. Completion note: Implemented JWT verification middleware with RS256 support. All 8 unit tests passing. Manual verification: auth flow works in dev. Created src/middleware/auth.ts (120 lines) and tests/middleware/auth.spec.ts (8 tests)."
```

## Never Mark Complete If

- Tests are failing
- Implementation is partial
- Unresolved errors exist
- Required files not found
- Blockers prevent verification

## Blocked Task Handling

If you cannot complete:

```bash
# Mark as blocked with reason
mcp__plugin_foundry_foundry-mcp__task action="block" spec_id={spec-id} task_id={task-id} reason="Missing API endpoint from task-2-1" blocker_type="dependency"

# Present alternatives to user
AskUserQuestion: "Task blocked. Options: 1) Resolve blocker first, 2) Work on different task, 3) Skip and document"
```
