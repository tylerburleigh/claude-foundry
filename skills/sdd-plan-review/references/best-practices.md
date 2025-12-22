# Best Practices

## DO

- Verify available tools before starting review
- Select review type based on spec risk/complexity
- Present findings organized by priority
- Note reviewer consensus in report
- Provide actionable recommendations
- Return report paths for full context
- Handle partial failures gracefully

## DON'T

- Skip tool verification step
- Use full review for trivial specs
- Ignore single-model findings entirely
- Modify specifications (use `sdd-modify`)
- Echo full review content to conversation
- Make up findings not from reviewer output
- Override priority based on personal judgment

## When to Escalate

Consider escalating to human review when:
- All models return conflicting views
- Critical security findings detected
- Architectural decisions affect multiple systems
- Estimates significantly exceed resources
- Compliance or regulatory concerns raised

---

## Integration with SDD Workflow

### Typical Workflow

```
1. sdd-plan creates specification
2. sdd-plan-review provides feedback (this skill)
3. sdd-modify applies approved changes
4. sdd-plan-review re-reviews if needed
5. sdd-next begins implementation
```

### Handoff to sdd-modify

After review, findings can flow to `sdd-modify`:
1. Human reviews findings and approves changes
2. `sdd-modify` applies approved modifications
3. Re-review confirms changes addressed findings

### Skip Conditions

Skip plan review when:
- Spec has <5 tasks and uses standard patterns
- Implementation already underway
- Exploratory/prototype work
- Simple CRUD operations
- Time constraints don't allow review
