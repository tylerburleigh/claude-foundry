# Output Format

## Markdown Report Structure

```markdown
## Feedback Summary

### Risk Flags (CRITICAL)
1. Missing authentication on admin endpoints
   Priority: CRITICAL | Flagged by: gemini, codex
   Impact: Unauthorized access to sensitive operations
   Recommendation: Add JWT validation middleware

### Feasibility Questions (HIGH)
2. Time estimates may be unrealistic for Phase 2
   Priority: HIGH | Flagged by: codex
   Impact: Timeline risk - OAuth integration underestimated
   Recommendation: Revisit estimates (+50% suggested)

### Design Concerns (MEDIUM)
3. Tight coupling between auth and user services
   Priority: MEDIUM | Flagged by: gemini
   Impact: Maintenance complexity
   Recommendation: Introduce interface abstraction

### Enhancement Suggestions (LOW)
4. Consider adding request logging
   Priority: LOW | Flagged by: cursor-agent
   Impact: Debugging and monitoring
   Recommendation: Add structured logging middleware
```

## JSON Summary Structure

```json
{
  "spec_id": "user-auth-001",
  "review_type": "full",
  "models_consulted": 3,
  "summary": {
    "critical": 1,
    "high": 3,
    "medium": 5,
    "low": 2
  },
  "findings": [
    {
      "id": 1,
      "category": "risk_flag",
      "priority": "critical",
      "title": "Missing authentication on admin endpoints",
      "flagged_by": ["gemini", "codex"],
      "impact": "Unauthorized access to sensitive operations",
      "recommendation": "Add JWT validation middleware"
    }
  ],
  "consensus": {
    "strong_consensus": 1,
    "diverse_perspectives": 4,
    "conflicting_views": 0
  }
}
```

## Output File Locations

| Type | Path | Purpose |
|------|------|---------|
| Markdown | `specs/.reviews/{spec-id}-review-{type}.md` | Human-readable report |
| JSON | `specs/.reviews/{spec-id}-review-{type}.json` | Machine-readable data |

---

## Output Isolation

### Why Isolation Matters

Review output can be extensive (hundreds of lines). Writing directly to the conversation wastes tokens and clutters context.

### Isolation Behavior

1. Full results go to files, not conversation
2. Conversation receives summary only (counts + paths)
3. Callers access full data via file paths

### Output Locations

- Markdown report: `specs/.plan-reviews/{plan-name}-{review-type}.md`
- JSON summary: `specs/.plan-reviews/{plan-name}-{review-type}.json`
