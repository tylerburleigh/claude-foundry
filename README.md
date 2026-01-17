# Claude Foundry - Spec-Driven Development for Claude Code

Plan before you code. Verify against the spec. Ship with confidence.

## Overview

Ad-hoc coding with AI assistants often leads to unclear requirements, untracked progress, and implementation drift. Claude Foundry brings structure to AI-assisted development through **spec-driven development (SDD)**: you create a detailed specification first, then implement against it with granular task tracking and AI-powered verification.

**Who it's for:** Developers using Claude Code who want traceable, auditable AI-assisted development with clear requirements and verifiable outcomes.

## Table of Contents

- [Key Features](#key-features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Example Workflow](#example-workflow)
- [How It Works](#how-it-works)
- [Skills Reference](#skills-reference)
- [Commands Reference](#commands-reference)
- [Configuration](#configuration)
- [Scope and Limitations](#scope-and-limitations)
- [Documentation](#documentation)
- [Support](#support)
- [License](#license)

## Key Features

- **Plan-First Workflow** - Create detailed specifications with phases, tasks, and acceptance criteria before writing code. AI-assisted review validates your plan.
- **Granular Task Tracking** - Track progress at the task level with states (pending, in_progress, completed, blocked), completion notes, and journal entries.
- **AI-Powered Verification** - Review implementation against spec requirements. Catch drift between what was planned and what was built.
- **Test Integration** - Run tests with structured debugging. AI consultation helps diagnose failures with 5-phase investigation.
- **Context-Rich PRs** - Generate pull request descriptions from spec metadata, journal entries, and git history. No more blank PR descriptions.
- **LSP-Powered Refactoring** - Safe symbol renaming and extraction with impact analysis. See what will change before committing.
- **Multi-Model Research** - Query multiple AI models in parallel for consensus, run deep web research, or brainstorm with structured ideation.

## Prerequisites

| Requirement | Version | Notes |
|-------------|---------|-------|
| Python | 3.10+ | Required for foundry-mcp server |
| Claude Code | Latest | The CLI tool from Anthropic |
| foundry-mcp | Latest | MCP server (installed via pip) |

## Installation

### 1. Install foundry-mcp

```bash
pip install foundry-mcp
```

Or with pipx for isolated installation:

```bash
pipx install foundry-mcp
```

Verify installation:

```bash
python -m foundry_mcp.server --help
```

### 2. Install the plugin

From within Claude Code:

```
/plugin marketplace add tylerburleigh/claude-foundry
/plugin install foundry@claude-foundry
```

Restart Claude Code and trust the repository when prompted.

> **Note:** The MCP server is automatically registered when you install the plugin. Do not manually add it with `claude mcp add`.

## Quick Start

1. **Run setup** - Configure permissions and verify installation:
   ```
   /setup
   ```

2. **Create a spec** - Plan your feature before coding:
   ```
   /sdd-plan
   ```

3. **Implement tasks** - Work through the spec task by task:
   ```
   /implement
   ```

4. **Verify and ship** - Review implementation, run tests, create PR:
   ```
   /sdd-review
   /run-tests
   /sdd-pr
   ```

## Example Workflow

A complete spec-driven development cycle:

```bash
# 1. Plan: Create a specification for your feature
/sdd-plan
# Claude asks clarifying questions, then generates a spec with phases and tasks
# Spec is saved to specs/pending/your-feature.json

# 2. Activate: Move spec to active when ready to implement
# (sdd-plan handles this automatically after review)

# 3. Implement: Work through tasks one at a time
/implement
# Shows next actionable task, provides context, tracks completion

# 4. Verify: Check implementation matches the spec
/sdd-review
# AI analyzes code against spec requirements, reports deviations

# 5. Test: Run tests and debug any failures
/run-tests
# Runs test suite, investigates failures with AI assistance

# 6. Ship: Create a PR with full context
/sdd-pr
# Generates PR description from spec, journals, and git history
```

## How It Works

```
sdd-plan → sdd-implement → [CODE] → sdd-review → run-tests → sdd-pr
   │            │              │          │           │          │
   ▼            ▼              ▼          ▼           ▼          ▼
 Create      Find next      Write     Verify      Validate   Generate
  spec        task          code      against      with       PR with
              and                      spec        tests     context
             track
```

| Phase | What happens |
|-------|--------------|
| **sdd-plan** | Creates a structured spec with phases, tasks, and acceptance criteria. AI review validates the plan. |
| **sdd-implement** | Finds the next actionable task, provides context, and tracks progress. Journals decisions. |
| **Coding** | You write the code. Foundry tracks which task you're working on. |
| **sdd-review** | Compares implementation against spec requirements. Reports compliance and drift. |
| **run-tests** | Executes test suite with structured debugging for failures. |
| **sdd-pr** | Creates PR description from spec metadata, journals, and commit history. |

## Skills Reference

Invoke skills with `/skill-name` or via the Skill tool.

| Skill | Purpose |
|-------|---------|
| `sdd-plan` | Create, review, and modify specifications |
| `sdd-implement` | Find next task, implement, track progress |
| `sdd-review` | Verify implementation matches specification |
| `run-tests` | Run tests with AI-assisted debugging |
| `sdd-pr` | Create PRs with spec context |
| `sdd-refactor` | LSP-powered refactoring with impact analysis |
| `research` | Multi-model research: chat, consensus, thinkdeep, ideate, deep |
| `bikelane` | Quick capture for ideas and issues |

## Commands Reference

| Command | Purpose |
|---------|---------|
| `/setup` | First-time setup and permissions configuration |
| `/implement` | Resume or start spec-driven development |
| `/tutorial` | Interactive tutorial for learning SDD |

## Configuration

The plugin stores configuration in `foundry-mcp.toml`. Key settings:

| Setting | Default | Description |
|---------|---------|-------------|
| `workspace` | Current directory | Working directory for specs |
| `specs_dir` | `specs/` | Where specs are stored |
| `providers` | System default | AI providers for research workflows |

Specs are organized by status:

```
specs/
├── pending/    # Specs in planning phase
├── active/     # Specs being implemented
└── completed/  # Finished specs (archived)
```

## Scope and Limitations

**Best for:**
- Feature development with clear requirements
- Bug fixes that benefit from structured investigation
- Multi-phase projects where progress tracking matters
- Work that needs auditable history (journals, specs)

**Not for:**
- One-line fixes or trivial changes
- Exploratory prototyping without clear goals
- Tasks where overhead exceeds benefit

## Documentation

| Guide | Description |
|-------|-------------|
| [Quick Start](docs/01-quick-start.md) | Get started in 5 minutes |
| [Core Concepts](docs/02-core-concepts.md) | Understand specs, phases, tasks |
| [Workflow Guide](docs/03-workflow-guide.md) | The complete SDD workflow |
| [Tutorial](docs/04-tutorial.md) | Build a feature from start to finish |
| [Command Reference](docs/05-command-reference.md) | All commands and options |
| [Troubleshooting](docs/06-troubleshooting.md) | Common issues and solutions |

## Support

- **Issues:** [GitHub Issues](https://github.com/tylerburleigh/claude-foundry/issues)
- **Bugs:** Open an issue with steps to reproduce
- **Features:** Open an issue describing the use case

## License

[MIT](LICENSE)
