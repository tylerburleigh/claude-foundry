# Review Source Workflows

How to apply modifications from different review sources.

## From sdd-plan-review

After plan quality review:

```bash
# Review generates report
# ... (sdd-plan-review output) ...

# Parse into modifications
mcp__plugin_foundry_foundry-mcp__review action="parse-feedback" spec_id="my-spec" review_path="reports/plan-review.md"

# Preview and apply
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="reports/plan-review.md.suggestions.json" dry_run=true
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="reports/plan-review.md.suggestions.json"
```

## From sdd-fidelity-review

After implementation fidelity check:

```bash
# Fidelity review generates report
# ... (sdd-fidelity-review output) ...

# Parse into modifications
mcp__plugin_foundry_foundry-mcp__review action="parse-feedback" spec_id="my-spec" review_path="reports/fidelity-review.md"

# Preview and apply
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="reports/fidelity-review.md.suggestions.json" dry_run=true
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="reports/fidelity-review.md.suggestions.json"
```

## Direct JSON Modification

When you know the exact changes:

```bash
# Create modifications.json manually
# Preview
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="modifications.json" dry_run=true

# Apply
mcp__plugin_foundry_foundry-mcp__spec action="apply-plan" spec_id="my-spec" modifications_file="modifications.json"
```
