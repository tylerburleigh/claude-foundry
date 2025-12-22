# Review Type Selection

## Quick Reference

| Type | Models | Duration | Focus | Use When |
|------|--------|----------|-------|----------|
| **quick** | 2 | 10-15 min | Completeness, Clarity | Simple specs, time-constrained |
| **full** | 3-4 | 20-30 min | All 6 dimensions | Complex specs, moderate-high risk |
| **security** | 2-3 | 15-20 min | Risk Management | Auth, data handling, compliance |
| **feasibility** | 2-3 | 10-15 min | Estimates, Dependencies | Tight deadlines, uncertain scope |

## Decision Matrix

| Condition | Review Type |
|-----------|-------------|
| Task count ≤ 10 AND no auth/payment/PII tasks | `quick` |
| Task count > 10 OR has architectural decisions | `full` |
| Contains auth, payment, encryption, or PII handling | `security` |
| Phase has deadline pressure OR external dependencies | `feasibility` |

## Priority Rules

When multiple conditions match:
1. `security` (always takes precedence for sensitive data)
2. `full` (for complex specs)
3. `feasibility` (for time-constrained work)
4. `quick` (default fallback)
