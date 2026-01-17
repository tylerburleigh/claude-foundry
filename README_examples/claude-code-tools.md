# Claude Code Tools - Complete README

This comprehensive guide covers productivity tools designed for Claude Code and similar CLI coding agents. The toolkit includes four primary commands: `aichat` for session management and search, `tmux-cli` for terminal automation, `vault` for encrypted .env backup, and `env-safe` for secure environment inspection.

## Core Features

**aichat** provides session continuation without lossy compaction through three strategies: trim (truncating large outputs), smart-trim (AI-powered selective truncation), and rollover (handing off to fresh sessions with lineage preservation). The tool includes fast full-text search via Rust/Tantivy with both human-friendly TUI and agent-compatible JSON output modes.

**tmux-cli** enables programmatic terminal control—described as "Playwright for terminals"—allowing Claude Code to test interactive scripts, coordinate debugging sessions, and facilitate inter-agent communication.

Additional utilities include `lmsh` (experimental natural-language shell), `vault` (centralized encrypted .env management), `env-safe` (safe environment variable inspection), and safety hooks preventing destructive git/docker/file operations.

## Installation & Setup

Install via `uv tool install claude-code-tools`, then optionally add the Rust search engine through Homebrew, Cargo, or pre-built binaries. The package includes pre-installed Node.js dependencies—no separate npm installation required.

Multiple Claude Code plugins extend functionality: `aichat` (session hooks/commands), `tmux-cli` (terminal automation skill), `workflow` (progress logging), `safety-hooks` (protective barriers), and `langroid` (multi-agent patterns).

## Advanced Capabilities

The lineage system preserves complete session history through parent-session pointers, enabling on-demand context retrieval without permanent information loss. Session-searcher sub-agents allow programmatic access to historical work via JSON query results.

Integration with alternative LLM providers (Kimi, Deepseek, local llama.cpp models) works through environment variable overrides. Google Docs tools (`md2gdoc`, `gdoc2md`) streamline markdown-to-Docs conversion workflows.

Development uses Python (CLI/logic), Rust (search TUI), and Node.js (action menus), with comprehensive Make targets for testing and publishing across PyPI and crates.io.

---
Source: https://github.com/pchalasani/claude-code-tools
