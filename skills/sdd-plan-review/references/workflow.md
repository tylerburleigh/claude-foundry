# Core Workflow (Detailed)

## Step 1: Verify Available Tools

```bash
mcp__plugin_foundry_foundry-mcp__review action="list-tools"
```

Check which AI review toolchains are installed:
- Need at least 1 tool for basic review
- 2+ tools recommended for multi-model feedback
- All 3 tools ideal for comprehensive analysis

## Step 2: Select Review Type

Use this decision matrix:

| Condition | Review Type |
|-----------|-------------|
| Task count ≤ 10 AND no auth/payment/PII tasks | `quick` |
| Task count > 10 OR has architectural decisions | `full` |
| Contains auth, payment, encryption, or PII handling | `security` |
| Phase has deadline pressure OR external dependencies | `feasibility` |

**Priority when multiple conditions match:**
1. `security` (always takes precedence for sensitive data)
2. `full` (for complex specs)
3. `feasibility` (for time-constrained work)
4. `quick` (default fallback)

## Step 3: Execute Review

```bash
mcp__plugin_foundry_foundry-mcp__review action="spec-review" spec_id="{spec-id}" review_type="{type}"
```

The tool automatically:
1. Initiates each model review in parallel
2. Collects responses as they complete (60-120s per tool)
3. Handles failures gracefully
4. Parses and synthesizes responses

## Step 4: Interpret Results

**Priority Levels:**
- **CRITICAL**: Security vulnerabilities, blockers → Address immediately
- **HIGH**: Design flaws, missing info → Address before implementation
- **MEDIUM**: Improvements, unclear requirements → Consider addressing
- **LOW**: Nice-to-have enhancements → Note for future

See `references/consensus.md` for priority assignment details.

## Step 5: Synthesize & Report

1. Organize feedback by category and priority
2. Note reviewer consensus and diverse perspectives
3. Identify type of downstream work needed
4. Return report path for full context

## Step 6: Handoff

After completing the review:

1. Present summary to user with finding counts by priority level
2. Provide paths to full reports (markdown + JSON)
3. Ask: "Would you like me to apply any of these recommendations using sdd-modify?"
4. Do NOT automatically proceed to sdd-modify without user approval
