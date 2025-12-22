# Best Practices and Reference

Guidelines, output format, and command reference.

## Best Practices

### DO

- **Always preview** before applying modifications
- **Use review-parse-feedback** for review reports
- **Validate** after every modification
- **Document** modifications in journal entries
- **Use batch_update** for multiple similar changes
- **Keep modifications atomic** - one logical change per apply

### DON'T

- **Skip** the dry-run preview step
- **Apply** modifications without understanding the scope
- **Continue** after validation failures
- **Manually edit** spec JSON files
- **Make multiple separate** modifications when batch_update would work
- **Ignore** validation warnings

---

## Output Format

Return a structured summary after modifications:

```markdown
## Modification Summary

**Spec:** {spec-id}
**Source:** {review report or modifications file}

### Changes Applied
- Updated X task descriptions
- Added Y verification steps
- Modified Z metadata fields

### Backup Location
`specs/.backups/{spec-id}-{timestamp}.json`

### Validation Status
{PASSED | FAILED with details}

### Next Steps
- {Contextual recommendations}
```

---

## Command Reference

### Core Commands

| Router | Action | Purpose |
|--------|--------|---------|
| `spec` | `apply-plan` | Apply modifications from JSON file |
| `review` | `parse-feedback` | Convert review markdown to modifications |
| `spec` | `validate` | Validate spec structure |
| `spec` | `validate-fix` | Auto-fix validation issues |

### Supporting Commands

| Router | Action | Purpose |
|--------|--------|---------|
| `spec` | `get` | Retrieve spec data |
| `spec` | `get-hierarchy` | Get spec structure |
| `spec` | `list` | List available specs |
| `authoring` | `task-add` | Add new task node |
| `authoring` | `task-remove` | Remove task node |
| `task` | `update-metadata` | Update task metadata |
| `authoring` | `update-frontmatter` | Update spec metadata |
| `verification` | `add` | Add verification result |
| `journal` | `add` | Add journal entry |

### Common Flags

| Flag | Description |
|------|-------------|
| `dry_run=true` | Preview without applying |
| `cascade=true` | Include child nodes in removal |
| `auto_fix=true` | Automatically fix validation issues |
