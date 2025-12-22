# Issue to Fix Mapping

## Auto-Fixable Issues

| Issue | Fix Action | Notes |
|-------|------------|-------|
| Incorrect task counts | Recalculates from hierarchy | Safe, no data loss |
| Missing metadata blocks | Adds empty metadata | Adds `{}` where missing |
| Orphaned nodes | Reconnects to parent | Uses position hints |
| Malformed timestamps | Converts to ISO 8601 | Preserves date/time info |
| Invalid status values | Resets to `pending` | Logs original value |
| Parent/child mismatches | Corrects references | Bi-directional sync |
| Missing required fields | Adds with defaults | Uses schema defaults |

## Manual-Only Issues

| Issue | Why Manual | Resolution |
|-------|-----------|------------|
| Circular dependencies | Requires intent understanding | Remove weakest edge |
| Orphaned dep references | Typo vs intentional | Fix typo or remove ref |
| Semantic inconsistencies | Business logic | Review spec intent |
| Custom metadata errors | Unknown structure | Requires context |
