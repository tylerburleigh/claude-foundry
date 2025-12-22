# Troubleshooting

## Auto-fix Succeeded But Errors Remain

This is normal. `mcp__plugin_foundry_foundry-mcp__spec action="fix"` reports success when it applies fixes successfully, not when all issues are resolved. Fixing one problem often reveals another (e.g., fixing parent-child mismatches may reveal orphaned nodes).

**Solution:** Re-validate to see remaining issues, then run fix again or address manually.

## Error Count Plateau

When error count stays the same for 2+ passes:

1. Run `mcp__plugin_foundry_foundry-mcp__spec action="validate" spec_id="{spec-id}"` to see detailed issue information
2. Identify issues marked "requires manual intervention"
3. Manually fix issues that need context or human judgment
4. Re-validate after manual fixes

**Understanding Spec Requirements:**
- Run `mcp__plugin_foundry_foundry-mcp__spec action="schema-export"` to see the complete spec structure, required fields, and valid values
- The schema shows all field types, enum values (like `status`, `type`, `verification_type`), and optional vs required fields
- Use this when validation errors reference unknown fields or invalid values

**Common manual-only issues:**
- Circular dependencies (remove one dependency edge)
- Orphaned dependencies (fix task ID typos)
- Logical inconsistencies (requires understanding spec intent)
- Custom metadata problems

## How Many Passes Are Normal?

- **2-3 passes**: Typical for most specs
- **5+ passes**: May indicate circular dependencies or structural issues

**Rule:** If error count decreases each pass, keep going. If it plateaus, switch to manual.

---

## Advanced Troubleshooting

### Validation Passes But Skill Fails

The spec may be structurally valid but semantically incorrect:
- Check that task IDs follow naming conventions
- Verify dependencies reference actual tasks
- Confirm parent-child relationships are logical

### Large Spec Performance

For specs with 100+ tasks:
- Use `--quiet` to suppress progress output
- Run validation in phases (validate -> fix -> re-validate)
- Consider splitting very large specs into multiple smaller ones

### Schema Validation Errors

If you see "JSON Schema validation failed":
1. Run `mcp__plugin_foundry_foundry-mcp__spec action="schema-export"` to see valid structure
2. Compare your spec against the schema
3. Look for required fields that are missing
4. Check enum values match allowed options
