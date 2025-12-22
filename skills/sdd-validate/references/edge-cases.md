# Edge Cases

## Empty Spec Files

Validation will fail with "missing required fields". Use `sdd-plan` to create new specs rather than manual creation.

## Backup Files

`mcp__plugin_foundry_foundry-mcp__spec action="fix"` creates `.backup` files before modification. To revert:
```bash
cp my-spec.json.backup my-spec.json
```

## CI/CD Integration

Use exit codes for automation:
- Exit 0: Pass
- Exit 1: Warning (may be acceptable)
- Exit 2: Error (fail the build)
- Exit 3: File error (fail the build)
