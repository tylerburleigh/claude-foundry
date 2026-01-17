# Command Reference

Detailed documentation for all Claude Foundry commands and skills.

## Commands vs Skills

| Type | How to invoke | Example |
|------|--------------|---------|
| **Commands** | Slash commands in chat | `/implement` |
| **Skills** | Ask Claude to use them | "Use sdd-plan to..." |

Commands are shortcuts that invoke skills or MCP-backed workflows with common configurations.

---

## Commands

### /setup

First-time configuration and verification.

**Usage:**
```
/setup [--check]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--check` | Only run verification, don't configure |

**What it does:**
1. Verifies foundry-mcp is installed
2. Checks Python and Git availability
3. Creates workspace directories (`specs/`)
4. Configures foundry-mcp.toml if needed
5. Verifies permissions

**Example:**
```
/setup --check

Pre-flight check results:
✓ foundry-mcp installed (v1.2.0)
✓ Python 3.11 available
✓ Git repository detected
✓ specs/ directory exists
✓ Permissions configured

All checks passed!
```

---

### /implement

Start or resume task implementation.

**Usage:**
```
/implement [--auto] [--delegate] [--parallel] [--model MODEL]
```

**Options:**
| Option | Description |
|--------|-------------|
| `--auto` | Reduce prompts between tasks |
| `--delegate` | Use subagent for each task |
| `--parallel` | Run multiple subagents concurrently (requires `--delegate`) |
| `--model MODEL` | Model for delegation: `haiku`, `sonnet`, `opus` |

Defaults come from `[implement]` in `foundry-mcp.toml`. If that section is absent, `/implement` runs in interactive inline mode; `/setup` recommends that default.

**Execution modes:**

| Mode | Flags | Best for |
|------|-------|----------|
| Interactive | (none) | Learning, complex work |
| Autonomous | `--auto` | Routine tasks, faster flow |
| Delegated | `--delegate` | Many tasks, fresh context per task |
| Parallel | `--delegate --parallel` | Independent tasks, max speed |

**Examples:**
```bash
# Interactive (default when not configured)
/implement

# Faster with less prompting
/implement --auto

# Delegate to subagents with Sonnet
/implement --delegate --model sonnet

# Maximum parallelism
/implement --delegate --parallel
```

**What happens:**
1. Finds active spec (or asks which to work on)
2. Recommends next task based on dependencies
3. Shows task details and acceptance criteria
4. Implements task (you or subagent)
5. Marks complete with journal entry
6. Offers to continue to next task

---

### /bikelane

Quick capture for ideas and issues.

**Usage:**
```
/bikelane [add|list|dismiss] [ITEM]
```

**Actions:**
| Command | Description |
|---------|-------------|
| `/bikelane <title>` or `/bikelane add <title>` | Capture a new item |
| `/bikelane` | Prompt for a title |
| `/bikelane list` | Show pending items |
| `/bikelane dismiss ID` | Mark item resolved |

**Item types:**
Use prefixes to categorize:
- `[Bug]` - Issues to fix
- `[Feature]` - Ideas for features
- `[Docs]` - Documentation improvements
- `[Idea]` - General ideas
- `[Error]` - Errors encountered

**Examples:**
```bash
# Capture an idea
/bikelane add [Feature] Add rate limiting to auth endpoints

# List pending items
/bikelane list

# Dismiss a resolved item
/bikelane dismiss item-001
```

**Why use bikelane?**
- Capture without interrupting flow
- Review ideas later when appropriate
- Don't lose track of small improvements

---

### /research

AI-powered research workflows.

**Usage:**
```
/research [WORKFLOW] [PROMPT]
/research thread-ID [FOLLOW-UP]
/research sessions [list|get|delete|status|report]
```

**Workflows:**
| Workflow | Description | Use for |
|----------|-------------|---------|
| `chat` | Single-model conversation | Quick questions, iteration |
| `consensus` | Multiple AI perspectives | Design decisions, trade-offs |
| `thinkdeep` | Systematic investigation | Complex debugging, analysis |
| `ideate` | Creative brainstorming | Exploring options |
| `deep` | Web research (background) | Comprehensive research |

**Examples:**
```bash
# Quick question
/research chat What's the best way to handle JWT refresh?

# Get multiple perspectives
/research consensus Should we use Redis or PostgreSQL for sessions?

# Systematic analysis
/research thinkdeep Why is this test flaky?

# Creative brainstorming
/research ideate How could we improve the onboarding flow?

# Deep research (runs in background)
/research deep Current best practices for API rate limiting 2025

# Resume a previous conversation
/research thread-abc123 What about the security implications?

# Check status of deep research
/research sessions status research-xyz789
```

**Session management:**
```bash
# List all research sessions
/research sessions list

# Get a specific session
/research sessions get research-xyz789

# Delete a session
/research sessions delete research-xyz789
```

---

## Skills

Skills are invoked by asking Claude to use them. They can also be invoked directly with `Skill(foundry:skill-name)`.

### sdd-plan

Create and manage specifications.

**When to use:**
- Starting a new feature
- Complex bug fixes
- Refactoring projects
- Any work benefiting from upfront planning

**Workflow stages:**
1. **Understand** - Clarify requirements
2. **Analyze** - Explore codebase
3. **Plan** - Create markdown plan
4. **Review** - AI review for issues
5. **Approve** - Human approval gate
6. **Create** - Convert to JSON spec
7. **Activate** - Move to active

**Invoking:**
```
Use sdd-plan to create a spec for user authentication.
```

Or describe the feature and let Claude recognize the need:
```
I want to add user authentication with JWT tokens.
```

**Key actions:**
| Action | What it does |
|--------|--------------|
| Create plan | Generate markdown plan |
| Review plan | Run AI review |
| Create spec | Convert plan to JSON |
| Modify spec | Update existing spec |
| Validate spec | Check for issues |
| Activate spec | Move to active |

---

### sdd-implement

Task execution and progress tracking.

**When to use:**
- After a spec is activated
- When resuming work
- To check what's next

**Usually invoked via `/implement` command**, but can be invoked directly:
```
Use sdd-implement to work on the next task.
```

**Task lifecycle:**
```
pending → in_progress → completed
                     ↘ blocked
```

**Key actions:**
| Action | What it does |
|--------|--------------|
| Prepare | Find next recommended task |
| Start | Mark task in_progress |
| Complete | Mark task completed |
| Block | Mark task blocked |
| Add dependency | Link tasks |

---

### sdd-review

Verify implementation matches specification.

**When to use:**
- After completing a phase
- Before final testing
- To check implementation quality

**Invoking:**
```
Run sdd-review on phase 1.
Run sdd-review on the entire spec.
Use sdd-review to check task-2-3.
```

**Review types:**
| Type | Scope |
|------|-------|
| Task review | Single task |
| Phase review | All tasks in phase |
| Spec review | Entire specification |

**Deviation categories:**
| Category | Meaning |
|----------|---------|
| Exact Match | Code matches spec |
| Minor Deviation | Small difference |
| Major Deviation | Significant difference |
| Missing | Not implemented |

---

### run-tests

Execute tests and debug failures.

**When to use:**
- After implementation
- When tests are failing
- For systematic debugging

**Invoking:**
```
Run the tests.
Use run-tests to debug the failing tests.
```

**Failure categories:**
| Category | Typical fix |
|----------|------------|
| Assertion | Check logic |
| Exception | Fix throwing code |
| Import | Fix dependencies |
| Setup | Fix test config |
| Timeout | Optimize code |
| Flaky | Fix race conditions |

**AI consultation levels:**
| Level | When |
|-------|------|
| None | Simple failures |
| Chat | Moderate complexity |
| Consensus | Need perspectives |
| ThinkDeep | Complex debugging |

---

### sdd-pr

Create pull requests with full context.

**When to use:**
- When spec is complete
- After tests pass
- Ready to submit for review

**Invoking:**
```
Create a PR for this spec.
Use sdd-pr to create a pull request.
```

**Context sources:**
- Spec metadata (title, mission)
- Completed tasks
- Git commits
- Journal entries
- Spec evolution

**PR sections:**
- Summary
- Changes
- Technical approach
- Testing checklist

---

### sdd-refactor

Safe refactoring with LSP.

**When to use:**
- Renaming symbols
- Extracting functions
- Moving code
- Cleaning up

**Invoking:**
```
Rename 'oldFunction' to 'newFunction' across the codebase.
Extract this code block into a function called 'helper'.
Use sdd-refactor to clean up unused imports.
```

**Operations:**
| Operation | What it does |
|-----------|--------------|
| Rename | Change symbol name everywhere |
| Extract function | Pull code into new function |
| Extract method | Pull code into class method |
| Move | Move symbol to different file |
| Dead code | Find and remove unused code |

**Safety features:**
- Shows impact before changes
- Requires approval for large refactors
- Verifies all references updated

---

## Configuration

### foundry-mcp.toml

Project-level configuration file.

**Location:** Project root

**Key settings:**
```toml
[workspace]
specs_dir = "./specs"         # Where specs are stored

[implement]
auto = false                  # Default for --auto flag
delegate = false              # Default for --delegate flag
parallel = false              # Default for --parallel flag
model = "haiku"               # Model for delegation

[consultation]
priority = [
  "[cli]codex:gpt-5.2-codex",
  "[cli]opencode:gpt-5.2-codex",
  "[cli]gemini:pro",
  "[cli]cursor-agent:composer-1",
  "[cli]claude:opus",
]
default_timeout = 300         # Seconds for AI calls

[research]
default_provider = "[cli]codex:gpt-5.2-codex"
```

---

## Quick Reference Card

### Common commands
```bash
/setup --check     # Verify setup
/implement         # Start/resume work
/implement --auto  # Less prompting
/bikelane add X    # Capture idea
/research chat X   # Quick research
```

### Common skill invocations
```
Create a spec for [feature]           → sdd-plan
Check what's next                     → sdd-implement
Verify implementation matches spec    → sdd-review
Run and debug tests                   → run-tests
Create a PR                           → sdd-pr
Rename X to Y                         → sdd-refactor
```

---

## Next Steps

- **[Troubleshooting](06-troubleshooting.md)** - When things don't work as expected
