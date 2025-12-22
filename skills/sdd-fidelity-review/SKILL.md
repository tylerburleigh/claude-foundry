---
name: sdd-fidelity-review
description: Review implementation fidelity against specifications by comparing actual code to spec requirements. Identifies deviations, assesses impact, and generates compliance reports for tasks, phases, or entire specs.
---

# Implementation Fidelity Review Skill

## Table of Contents

- [Overview](#overview)
- [Skill Family](#skill-family)
- [When to Use](#when-to-use-this-skill)
- [MCP Tooling](#mcp-tooling)
- [Core Workflow](#core-workflow)
- [LSP Operations Quick Reference](#lsp-operations-quick-reference)
- [Essential Commands](#essential-commands)
- [Review Types](#review-types)
- [Assessment Categories](#fidelity-assessment-categories)
- [Long-Running Operations](#long-running-operations)
- [Example Invocation](#example-invocation)

## Overview

The `sdd-fidelity-review` skill compares actual implementation against SDD specification requirements. It uses LSP for structural verification and MCP for AI-powered deviation analysis.

## Skill Family

Part of the **Spec-Driven Development** quality assurance family:

```
sdd-plan → sdd-next → Implementation → sdd-update → sdd-fidelity-review (this skill) → run-tests
```

## When to Use This Skill

**Use when:**
- Verifying implementation matches specification requirements
- Identifying deviations between plan and actual code
- Reviewing pull requests for spec compliance
- Auditing completed phases or tasks

**Do NOT use for:**
- Creating specifications (use `sdd-plan`)
- Finding next tasks (use `sdd-next`)
- Updating task status (use `sdd-update`)
- Running tests (use `run-tests`)

## MCP Tooling

This skill uses the Foundry MCP server with router+action pattern: `mcp__plugin_foundry_foundry-mcp__<router>` with `action="<action>"`.

**Critical Rules:**
- **ALWAYS** use MCP tools for spec operations
- **NEVER** use `Read()` on spec JSON files
- **NEVER** use shell commands (`cat`, `grep`, `jq`) on specs

## Core Workflow

The fidelity review workflow integrates LSP verification with MCP AI analysis:

### Step 1: Gather Context (Optional)

For unfamiliar code, use Explore subagent to find implementation files:
```
Explore agent (medium thoroughness): Find all files in phase-1, related tests, config files
```

### Step 2: LSP Structural Pre-Check

Before the AI review, verify structural requirements with LSP:

```python
# Get symbols in implementation file
symbols = LSP(operation="documentSymbol", filePath="src/auth/service.py", line=1, character=1)

# Compare against spec: expects AuthService with login(), logout(), refresh_token()
# Identify missing symbols before expensive AI review
```

**Why:** Catches missing implementations in seconds before 5-minute AI review.

### Step 3: MCP Fidelity Review

Run the AI-powered fidelity analysis:

```bash
# Phase review
mcp__plugin_foundry_foundry-mcp__review action="fidelity" spec_id="{spec-id}" phase_id="{phase-id}"

# Task review
mcp__plugin_foundry_foundry-mcp__review action="fidelity" spec_id="{spec-id}" task_id="{task-id}"
```

The MCP tool handles spec loading, implementation analysis, AI consultation, and report generation.

### Step 4: LSP-Assisted Investigation

For deviations found, use LSP to investigate:

```python
# Trace deviation origin
definition = LSP(operation="goToDefinition", filePath="src/auth/service.py", line=45, character=10)

# Find what depends on deviated code
calls = LSP(operation="incomingCalls", filePath="src/auth/service.py", line=45, character=10)

# Assess blast radius
refs = LSP(operation="findReferences", filePath="src/auth/service.py", line=45, character=10)
```

**Why:** Understand deviation impact before recommending fixes.

> For detailed LSP patterns and examples, see `reference.md#lsp-integration-patterns`

## LSP Operations Quick Reference

| Operation | When to Use | Purpose |
|-----------|-------------|---------|
| `documentSymbol` | Pre-check | List all symbols in a file for structural verification |
| `workspaceSymbol` | Pre-check | Find symbols across codebase |
| `hover` | During review | Get type info and documentation |
| `goToDefinition` | Investigation | Trace where symbols are defined |
| `findReferences` | Investigation | Find all usages of a symbol |
| `incomingCalls` | Investigation | Find what calls a function |
| `outgoingCalls` | Investigation | Find what a function calls |

## Essential Commands

**Query tasks before review:**
```bash
mcp__plugin_foundry_foundry-mcp__task action="query" spec_id="{spec-id}" parent="{phase-id}"
```

**Phase review:**
```bash
mcp__plugin_foundry_foundry-mcp__review action="fidelity" spec_id="{spec-id}" phase_id="{phase-id}"
```

**Task review:**
```bash
mcp__plugin_foundry_foundry-mcp__review action="fidelity" spec_id="{spec-id}" task_id="{task-id}"
```

> For query patterns and anti-patterns, see `reference.md#querying-spec-and-task-data-efficiently`

## Review Types

| Type | Scope | When to Use |
|------|-------|-------------|
| **Phase Review** | 3-10 tasks | Phase completion checkpoints |
| **Task Review** | 1 file | Critical task validation, high-risk implementations |

> For detailed workflow per review type, see `reference.md#review-types`

## Fidelity Assessment Categories

| Category | Meaning |
|----------|---------|
| **Exact Match** | Implementation precisely matches specification |
| **Minor Deviation** | Small differences with no functional impact |
| **Major Deviation** | Significant differences affecting functionality |
| **Missing** | Specified features not implemented |

## Long-Running Operations

**This skill may take up to 5 minutes.** The MCP tool handles timeout internally.

**Never** use `run_in_background=True` with frequent polling.

## Example Invocation

```bash
Skill(foundry:sdd-fidelity-review) "Review phase phase-1 in spec user-auth-001"
```

## Detailed Reference

For comprehensive documentation including complete workflows, report templates, error handling, and best practices, see **[reference.md](./reference.md)**
