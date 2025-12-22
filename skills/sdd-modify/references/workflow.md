# Workflow Details

Step-by-step workflows for applying spec modifications.

## Table of Contents

- [Parse Review Feedback](#workflow-1-parse-review-feedback)
- [Preview Modifications](#workflow-2-preview-modifications)
- [Apply Modifications](#workflow-3-apply-modifications)
- [Direct Modifications](#workflow-4-direct-modifications-without-review-parsing)

---

## Workflow 1: Parse Review Feedback

When starting from a review report (markdown output from sdd-plan-review or sdd-fidelity-review), convert it to structured modifications:

```bash
mcp__plugin_foundry_foundry-mcp__review action="parse-feedback" spec_id="my-spec-001" review_path="reports/plan-review.md"
```

**Output:** Creates a modifications JSON file (typically `{review-path}.suggestions.json`)

**Example output structure:**
```json
{
  "spec_id": "my-spec-001",
  "source": "reports/plan-review.md",
  "generated": "2025-10-18T14:30:00Z",
  "modifications": [
    {
      "operation": "update_task",
      "task_id": "task-2-1",
      "field": "description",
      "value": "Implement OAuth 2.0 with PKCE flow",
      "reason": "Plan review suggested more specific description"
    }
  ]
}
```

**When to use:**
- After running `sdd-plan-review` or `sdd-fidelity-review`
- When you have markdown review output that needs conversion
- When review findings need to be applied systematically

---

## Workflow 2: Preview Modifications

**Always preview before applying.** Use dry-run mode to see what will change:

```bash
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec-001" modifications_file="suggestions.json" dry_run=true
```

**Example preview output:**
```
Dry Run - Modifications Preview
================================

Spec: my-spec-001
Source: suggestions.json

Changes that would be applied:
1. UPDATE task-2-1.description
   Old: "Implement authentication"
   New: "Implement OAuth 2.0 with PKCE flow"

2. ADD verification to task-1-3
   Type: manual
   Description: "Verify token refresh works correctly"

3. UPDATE task-3-2.metadata.estimated_hours
   Old: 4
   New: 6

Summary: 2 task updates, 1 verification addition
```

**Review checklist:**
- [ ] All intended changes are listed
- [ ] No unexpected changes
- [ ] Task IDs are correct
- [ ] Values look correct

---

## Workflow 3: Apply Modifications

After confirming the preview, apply changes:

```bash
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec-001" modifications_file="suggestions.json"
```

**What happens automatically:**
1. **Backup created** at `specs/.backups/my-spec-001-{timestamp}.json`
2. **Modifications applied** in order
3. **Validation runs** on the modified spec
4. **Rollback occurs** if validation fails (original restored from backup)

**Success output:**
```
Modifications Applied Successfully
==================================

Spec: my-spec-001
Backup: specs/.backups/my-spec-001-20251018-143000.json

Applied:
- Updated 2 task descriptions
- Added 1 verification step
- Modified 1 metadata field

Validation: PASSED
```

---

## Workflow 4: Direct Modifications (without review parsing)

When you have pre-prepared modifications or want to make manual bulk changes:

**Step 1:** Create modifications JSON file directly:

```json
{
  "spec_id": "my-spec-001",
  "modifications": [
    {
      "operation": "update_task",
      "task_id": "task-1-2",
      "field": "details",
      "value": "Updated task description text"
    },
    {
      "operation": "add_verification",
      "task_id": "task-2-1",
      "verification": {
        "verification_type": "run-tests",
        "mcp_tool": "mcp__plugin_foundry_foundry-mcp__test action=\"run\"",
        "expected": "All tests pass"
      }
    }
  ]
}
```

**Step 2:** Preview and apply:

```bash
# Preview
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec-001" modifications_file="modifications.json" dry_run=true

# Apply
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec-001" modifications_file="modifications.json"
```

**When to use:**
- Bulk updates to multiple tasks
- Programmatic spec modifications
- Migrating specs with automated transformations
- When you know exactly what changes to make
