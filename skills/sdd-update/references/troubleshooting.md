# Troubleshooting

Common issues and their resolutions.

## Spec File Corruption

**Recovery:**
1. Check for backup: `specs/active/{spec-id}.json.backup`
2. If no backup, regenerate from original spec
3. Manually mark completed tasks based on journal entries
4. Validate repaired file

## Orphaned Tasks

**Resolution:**
1. If task in file but not in spec: Check if spec was updated; remove if confirmed deleted
2. If task in spec but not in file: Regenerate spec file using sdd-plan
3. Always preserve completed task history even if spec changed

## Merge Conflicts

**When:** Multiple tools update state simultaneously

**Resolution:**
1. Load both versions
2. Identify conflicting nodes
3. Choose most recent update (check timestamps)
4. Recalculate progress from leaf nodes up
5. Validate merged state

---

# Common Mistakes

## Using `--entry-type completion`

**Error:**
```bash
mcp__plugin_foundry_foundry-mcp__journal add: error: argument --entry-type: invalid choice: 'completion'
Exit code: 2
```

**Cause:** Confusing the `bulk-journal --template` option with `add-journal --entry-type`

**Fix:** Use `--entry-type status_change` instead:

```bash
# WRONG - "completion" is not a valid entry type
mcp__plugin_foundry_foundry-mcp__journal action="add" spec_id={spec-id} task_id={task-id} entry_type="completion" ...

# CORRECT - Use "status_change" for task completion entries
mcp__plugin_foundry_foundry-mcp__journal action="add" spec_id={spec-id} task_id={task-id} entry_type="status_change" title="Task Completed" content="..."
```

**Why this happens:** The entry_type parameter has specific valid values. Use `status_change` for task completion entries, `decision` for decisions, `deviation` for deviations, etc.

## Reading Spec Files Directly

**Error:** Using Read tool, cat, grep, or jq on spec files

**Fix:** Always use the standardized MCP tools:

```bash
# WRONG - Wastes context tokens and bypasses validation
Read("specs/active/my-spec.json")
cat specs/active/my-spec.json

# CORRECT - Use the MCP tool for structured access
mcp__plugin_foundry_foundry-mcp__spec action="get" spec_id={spec-id}
mcp__plugin_foundry_foundry-mcp__task action="info" spec_id={spec-id} task_id={task-id}
mcp__plugin_foundry_foundry-mcp__task action="query" spec_id={spec-id} status="pending"
```
