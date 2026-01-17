# claude-foundry

A Claude Code plugin for working with [foundry-mcp](https://github.com/tylerburleigh/foundry-mcp).

## Overview

This plugin provides skills, commands, and agents for spec-driven development (SDD) workflows powered by the Foundry MCP server.

**New to Claude Foundry?** Check out the [User Documentation](docs/README.md) for guides, tutorials, and examples.

## Documentation

| Guide | Description |
|-------|-------------|
| [Quick Start](docs/01-quick-start.md) | Get started in 5 minutes |
| [Core Concepts](docs/02-core-concepts.md) | Understand specs, phases, tasks |
| [Workflow Guide](docs/03-workflow-guide.md) | The complete SDD workflow |
| [Tutorial](docs/04-tutorial.md) | Build a feature from start to finish |
| [Command Reference](docs/05-command-reference.md) | All commands and options |
| [Troubleshooting](docs/06-troubleshooting.md) | Common issues and solutions |

## Prerequisites

### Install foundry-mcp (Required)

The plugin requires the `foundry-mcp` Python package to be installed:

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

> **Note:** The MCP server (`foundry-mcp`) is automatically registered when you install the plugin. Do not manually add it with `claude mcp add` as this would create a duplicate.

## Installation

### Install from Claude Code (Recommended)

From within Claude Code, add the marketplace and install the plugin:

```
/plugin marketplace add tylerburleigh/claude-foundry
/plugin install foundry@claude-foundry
```

Restart Claude Code and trust the repository when prompted. The plugin will auto-update when new versions are released.

## Plugin Structure

```
claude-foundry/
├── .claude-plugin/
│   └── marketplace.json    # Plugin registration
├── agents/                 # Subagent definitions
├── commands/               # Slash commands (/foundry:*)
├── hooks/                  # Pre/post tool use hooks
│   └── hooks.json
├── skills/                 # Skills (foundry:*)
└── README.md
```

## Available Components

### Skills (`foundry:*`)

Skills are invoked via `Skill(foundry:skill-name)`:

- `foundry:sdd-plan` - Create detailed specifications before coding (includes review, modification, validation)
- `foundry:sdd-implement` - Find next task, implement, and track progress
- `foundry:sdd-review` - Verify implementation matches specification
- `foundry:run-tests` - Run tests with debugging support
- `foundry:sdd-pr` - Create PRs with AI-enhanced descriptions
- `foundry:sdd-refactor` - LSP-powered refactoring with impact analysis

### Commands

Slash commands for common workflows:

- `/setup` - First-time setup and guided tour of plugin features
- `/implement` - Resume or start spec-driven development work
- `/tutorial` - Interactive tutorial for Spec-Driven Development

### Agents

Subagents for specialized tasks (spawned via Task tool).

## Configuration

The plugin requires the foundry-mcp MCP server to be installed and registered (see Prerequisites above). Once configured, the server starts automatically when Claude Code launches.

## Permissions

Claude Code requires explicit permissions for plugin tools. Run `/setup` to configure permissions automatically, or see `templates/settings.local.json.example` for the full list of required permissions.

### What's included

The sample config allows:

| Category | Permissions |
|----------|-------------|
| **MCP Tools** | All `mcp__plugin_foundry_foundry-mcp__*` tools |
| **Git** | `status`, `diff`, `log`, `rev-parse`, `branch`, `show` |
| **Testing** | `pytest`, `python -m pytest` |
| **Foundry CLI** | `foundry-mcp`, `foundry-cli` |
| **AI Providers** | `codex`, `claude`, `gemini`, `cursor-agent`, `opencode` |
| **File Access** | Write/Edit to `specs/` directories |
| **Denied** | Direct `Read` of spec JSON files (use MCP tools instead) |

### Customizing

Edit `.claude/settings.local.json` to add or remove permissions. See [Claude Code permissions docs](https://docs.anthropic.com/en/docs/claude-code) for the full syntax.

## License

MIT
