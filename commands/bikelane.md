---
name: bikelane
description: Fast-capture intake queue for ideas and follow-up tasks. Add items quickly, list pending items, or dismiss resolved ones.
argument-hint: [add|list|dismiss] [title or item-id]
---

# Bikelane Quick Capture

Fast intake of ideas/tasks without immediate triage overhead. Items are stored in `specs/.bikelane/intake.jsonl`.

## Routing by Argument

Parse $ARGUMENTS to determine action:

### Default: Quick Add

If arguments start with text (not `list` or `dismiss`), treat as a title to add:

```bash
mcp__plugin_foundry_foundry-mcp__intake action="add" title="$ARGUMENTS" source="bikelane-cmd"
```

If no arguments provided, prompt for title:
```
AskUserQuestion: "What would you like to capture?"
```

### `list`: Show Pending Items

```bash
mcp__plugin_foundry_foundry-mcp__intake action="list" limit=10
```

Present items with options via `AskUserQuestion`:
- "Dismiss item" - mark as dismissed
- "Show more" - fetch next page
- "Done" - exit

### `dismiss <item-id>`: Mark Item Dismissed

```bash
mcp__plugin_foundry_foundry-mcp__intake action="dismiss" item_id="$2"
```

Optionally ask for dismissal reason.

## Examples

| Command | Action |
|---------|--------|
| `/bikelane Add retry logic to API calls` | Quick add with title |
| `/bikelane` | Prompt for title |
| `/bikelane list` | Show pending items |
| `/bikelane dismiss intake-abc123` | Dismiss specific item |

## Optional Parameters for Add

When adding items, you may include additional context:

```bash
mcp__plugin_foundry_foundry-mcp__intake action="add" \
  title="Title here" \
  description="Longer description" \
  priority="p1" \
  tags='["tag1", "tag2"]' \
  source="bikelane-cmd"
```

Priority levels: `p0` (critical) → `p4` (backlog). Default is `p2` (medium).
