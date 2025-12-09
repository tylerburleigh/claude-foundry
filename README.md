# claude-foundry

A Claude Code plugin for working with [foundry-mcp](https://github.com/tylerburleigh/foundry-mcp).

## Overview

This plugin provides skills, commands, and agents for spec-driven development (SDD) workflows powered by the Foundry MCP server.

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

## Installation

### Easiest method (script)

Run from command line. Requires `jq`. Adds marketplace, pre-enables the plugin.

```bash
mkdir -p ~/.claude && \
  touch ~/.claude/settings.json && \
  [ ! -s ~/.claude/settings.json ] && echo '{}' > ~/.claude/settings.json; \
  jq '. * {
    "extraKnownMarketplaces": {
      "claude-foundry": {
        "source": {"source": "github", "repo": "tylerburleigh/claude-foundry"}
      }
    },
    "enabledPlugins": {
      "foundry@claude-foundry": true
    }
  }' ~/.claude/settings.json > ~/.claude/settings.json.tmp && \
  mv ~/.claude/settings.json.tmp ~/.claude/settings.json
```

Then restart Claude Code and trust the repository when prompted.

### Alternative: Install from within Claude Code

If you prefer, you can add the marketplace interactively from inside Claude Code:

```
/plugin marketplace add tylerburleigh/claude-foundry
```

Then run `/plugin` to browse and install available plugins.

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

- `foundry:sdd-plan` - Create detailed specifications before coding
- `foundry:sdd-next` - Find and prepare the next actionable task
- `foundry:sdd-update` - Track progress and update task status
- `foundry:run-tests` - Run tests with debugging support
- `foundry:sdd-validate` - Validate spec structure and dependencies
- `foundry:sdd-pr` - Create PRs with AI-enhanced descriptions
- `foundry:sdd-review` - Review specs for quality and issues

### Commands

Slash commands for common workflows:

- `/foundry-setup` - First-time setup and guided tour of plugin features
- `/sdd-next` - Resume or start spec-driven development work

### Agents

Subagents for specialized tasks (spawned via Task tool).

## Configuration

The plugin automatically configures the foundry-mcp MCP server when installed. The server starts automatically when the plugin is enabled - no manual MCP configuration needed.

If the MCP server fails to start, verify that `foundry-mcp` is installed (see Prerequisites above).

## License

MIT
