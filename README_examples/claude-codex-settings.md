# Claude Codex Settings - Complete README

This is a comprehensive configuration repository for Claude Code and OpenAI Codex featuring 15+ plugins with MCP servers, skills, agents, and custom commands.

## Core Offerings

**Installation via Plugin Marketplace:**
The setup uses Claude Code's plugin system to install modular tools including Azure, GitHub, Linear, MongoDB, Slack, Supabase, and Tavily integrations.

**Plugin Categories:**
- Cloud platforms (Azure, GCloud, Supabase)
- Development tools (GitHub workflows, Playwright testing, plugin development)
- Integrations (Slack, Linear, MongoDB, Tavily search)
- Utilities (notifications, statusline tracking, code formatting)

## Key Features

**Model Configuration:**
Supports OpusPlan mode with Opus 4.5 for planning and execution, plus alternative configurations for Z.ai GLM models (85% cost savings) and Kimi K2 thinking models.

**Skills & Agents:**
Includes autonomous agents for code simplification, commit creation, PR reviews, and skill development, with progressive disclosure UI patterns.

**Development Hooks:**
Auto-formatting for Python (Google-style docstrings), JavaScript/TypeScript, Markdown, and Bash with quality enforcement via ruff and Prettier.

**Statusline Plugin:**
Real-time account usage display showing context percentage, costs, and 5-hour block reset timing with color-coded status indicators.

## Setup Requirements

Prerequisites include Claude Code, required CLI tools, and configuration via `/plugin-name:setup` commands post-installation with symlink creation for cross-tool compatibility.

---
Source: https://github.com/fcakyon/claude-codex-settings
