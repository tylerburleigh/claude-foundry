---
name: foundry-setup
description: First-time setup for the claude-foundry plugin
argument-hint: [--check]
---

# Foundry Setup Command

When invoked, execute these phases in order. This command is idempotent and safe to run multiple times.

## Argument Handling

Check if `$ARGUMENTS` contains `--check` or `--preflight`:
- **If flag present:** Run only Phase 1 (Pre-flight Checks), display results, then stop
- **If no flag:** Run all phases (full setup)

## Execution

```
- **Entry** → Read `references/foundry-setup.md` (MANDATORY)
  - → Preflight (MCP, Python, Git)
    - [--check?] → **Exit**
  - → Permissions → Workspace
    - [toml created?] → Providers → TestConfig
  - → Summary → **Exit**
```
