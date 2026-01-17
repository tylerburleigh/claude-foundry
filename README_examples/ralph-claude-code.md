# Ralph for Claude Code - Complete README

**What It Is:**
Ralph is a bash-based autonomous development framework that enables continuous AI-powered coding cycles. Named after Geoffrey Huntley's technique, it leverages Claude Code to "iteratively improve your project until completion, with built-in safeguards."

**Key Features:**
- Autonomous development loops with dual-condition exit detection (requires both completion indicators AND explicit EXIT_SIGNAL)
- Rate limiting (100 calls/hour default) and circuit breaker patterns
- Session continuity across iterations with configurable 24-hour expiration
- Live tmux dashboard monitoring
- PRD/specification import conversion
- 308 comprehensive tests with 100% pass rate

**Installation Model:**
Two-phase setup: (1) System-wide installation via `./install.sh`, then (2) per-project initialization using `ralph-setup` or `ralph-import` commands.

**Current Status:**
Version 0.9.9 in active development, approaching v1.0 with planned additions including log rotation, dry-run mode, and configuration file support.

**Requirements:**
Bash 4.0+, Claude Code CLI, tmux (recommended), jq, and Git.

The project actively welcomes contributions across testing, documentation, and feature development.

---
Source: https://github.com/frankbria/ralph-claude-code
