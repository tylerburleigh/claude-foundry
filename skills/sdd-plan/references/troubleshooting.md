# Troubleshooting

Common issues and their resolutions.

## Validation Errors

**Error:** "Circular dependency detected"

**Resolution:**
1. Check `depends_on` arrays for cycles
2. Use `spec action="validate"` to identify specific tasks
3. Remove or reorder dependencies

**Error:** "Missing file_path for implementation task"

**Resolution:**
1. Add `metadata.file_path` to task
2. Or change `task_category` to `investigation`

**Error:** "Invalid task ID format"

**Resolution:**
- Follow pattern: `task-{phase}-{n}`, `verify-{phase}-{n}`
- Phase number must match parent phase

## Spec Not Found

**Symptoms:** MCP tools can't find spec

**Checks:**
1. Verify file exists: `ls specs/pending/` or `specs/active/`
2. Check spec_id matches filename (without .json)
3. Ensure valid JSON: `spec action="validate"`

## Plan Review Fails

**Symptoms:** `plan action="review"` returns error

**Checks:**
1. Plan file exists in `specs/.plans/`
2. Markdown is well-formed
3. Required sections present (Objective, Phases)

## LSP Not Available

**Symptoms:** LSP tools return errors

**Fallbacks:**
1. Use Explore agent with specific search terms
2. Use Grep for symbol search: `Grep pattern="class AuthService"`

---

# Quick Reference

## Creation Commands

```bash
# Create markdown plan
mcp__plugin_foundry_foundry-mcp__plan action="create" name="Feature Name" template="detailed"

# Review plan
mcp__plugin_foundry_foundry-mcp__plan action="review" plan_path="specs/.plans/feature-name.md" review_type="full"

# Create JSON spec
mcp__plugin_foundry_foundry-mcp__authoring action="spec-create" name="feature-name" template="medium"

# Validate spec
mcp__plugin_foundry_foundry-mcp__spec action="validate" spec_id="{spec-id}"

# Auto-fix validation errors
mcp__plugin_foundry_foundry-mcp__spec action="validate-fix" spec_id="{spec-id}" auto_fix=true
```

## Analysis Commands

```bash
# LSP tools (preferred)
documentSymbol(file="src/auth/service.py")
findReferences(file="src/auth/service.py", symbol="AuthService", line=15, character=6)
goToDefinition(file="src/api/routes.py", symbol="authenticate", line=42, character=10)
incomingCalls(file="src/auth/service.py", symbol="authenticate", line=42, character=10)

# Fallback: Grep
Grep pattern="class ClassName" type="py"
Grep pattern="def functionName" type="py"
```

## File Locations

| Artifact | Location |
|----------|----------|
| Markdown plans | `specs/.plans/` |
| Plan reviews | `specs/.plan-reviews/` |
| Pending specs | `specs/pending/` |
| Active specs | `specs/active/` |
| Completed specs | `specs/completed/` |
| Archived specs | `specs/archived/` |
