---
name: implement
description: Resume or start spec-driven development work by detecting active tasks and providing interactive options
---

# SDD Implement Command

When invoked, follow these steps:

## Step 0: Flag Parsing and Session Check

### Parse Command Flags

| Flag | Description | Mode |
|------|-------------|------|
| `--auto` | Autonomous execution - continue tasks without prompts | Autonomous |
| `--parallel` | Parallel execution - run independent tasks concurrently | Parallel |
| `--delegate` | Interactive with delegation - subagent implements each task | Delegated |
| (none) | Interactive single-task mode (default) | Interactive |

### Check for Existing Session

Before proceeding, check if a paused session exists:

```bash
mcp__plugin_foundry_foundry-mcp__task action="session-config" spec_id={spec-id} command="status"
```

**If session is `paused`:**
```
AskUserQuestion:
"Found paused session (reason: {pause_reason}). Last task: {current_task}"
Options:
- "Resume session" → session-config command="resume", then continue
- "Start fresh" → session-config command="end", then proceed normally
- "Exit"
```

**If session is `active`:**
Warn user that a session is already running. Offer to view status or exit.

**If session is `idle` or no session:**
Proceed to Step 1.

### Interactive Fallback (No Flags)

When no flags provided, offer mode selection:

```
AskUserQuestion:
"Select execution mode:"
Options:
- "Interactive (single task)" → Proceed to Step 1
- "Interactive with delegation (--delegate)" → Proceed to Step 1 with delegation enabled
- "Autonomous (--auto)" → Start autonomous session
- "Parallel (--parallel)" → Start parallel session
- "Exit"
```

## Step 1: Check for Active Specifications

Use `mcp__plugin_foundry_foundry-mcp__spec action="list"` with status "active" to find specifications in progress.

## Step 2: Route Based on Results

### If active specs exist:
Pass the active spec context to the `sdd-implement` skill. The skill will proceed directly to task selection - it does NOT need to re-check for active specs.

**IMPORTANT:** Do NOT invoke this command or skill recursively. The skill handles the full workflow.

```
Skill(sdd-implement) "Active spec detected via /implement command. Skip Step 3.1 (spec detection) and proceed directly to Step 3.2 (task selection)."
```

### If no active specs exist:
Check for pending specs using `mcp__plugin_foundry_foundry-mcp__spec action="list"` with status "pending".

**If pending specs found:**
Ask user if they want to activate one using `AskUserQuestion`.

**If no specs at all:**
Inform user no specifications exist and offer options:
1. Create a new spec using the sdd-plan skill
2. View completed/archived specs
3. Exit

## Step 3: Hand Off to Skill

Once routing is determined, the sdd-implement skill handles the full workflow:
- Task preparation and context gathering
- Implementation guidance
- Progress tracking and verification

This command is the user entry point; the skill contains the detailed workflow logic.
