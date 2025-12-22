# Error Handling and Troubleshooting

Common issues, constraints, and resolution strategies.

## Error Handling

### Spec Not Found

**Symptom:** Error mentioning spec ID not found

**Resolution:**
1. Verify spec ID is correct
2. Check spec location: `mcp__plugin_foundry_foundry-mcp__spec action="list"`
3. Ensure spec is in expected folder (active/, pending/, etc.)

### Invalid Modifications File

**Symptom:** JSON parse error or invalid operation

**Resolution:**
1. Validate JSON syntax
2. Check operation names are correct
3. Verify task IDs exist in spec
4. Ensure required fields are present

### Validation Failure After Apply

**Symptom:** "Validation failed, rolling back" message

**What happened:**
- Modifications were applied
- Validation detected issues
- Original spec restored from backup

**Resolution:**
1. Review validation errors
2. Fix modifications to address validation issues
3. Re-apply with corrected modifications

### Review Report Cannot Be Parsed

**Symptom:** `review-parse-feedback` fails

**Resolution:**
1. Verify review file exists and is readable
2. Check review format matches expected structure
3. Try running review again with proper output format

---

## Constraints

- **Do NOT** read spec JSON directly - use MCP tools only
- **Do NOT** apply modifications without previewing first
- **Do NOT** skip validation after apply
- **Do NOT** continue if validation fails - report and stop
- **Return** concise summaries, not verbose logs
- **Document** significant modifications in journal entries

---

## Troubleshooting

### Preview Shows No Changes

**Possible causes:**
- Modifications file is empty or malformed
- Task IDs don't exist in spec
- Values are already set to requested values

**Fix:** Verify modifications JSON structure and task IDs

### Validation Fails Repeatedly

**Possible causes:**
- Modifications create invalid spec structure
- Circular dependencies introduced
- Required fields missing

**Fix:** Review validation errors, adjust modifications accordingly

### Rollback Occurred But Issue Persists

**Possible causes:**
- Backup file was corrupted
- Manual edits made between backup and apply

**Fix:**
1. Check backup file integrity
2. Use older backup if available
3. Regenerate spec from source if necessary

### Review Parsing Produces Empty Modifications

**Possible causes:**
- Review report has no actionable findings
- Review format not recognized

**Fix:**
1. Check review report content
2. Ensure review follows expected markdown structure
3. Consider direct modifications if parsing fails
