# Claude Task Runner - Complete README

Claude Task Runner is a specialized CLI tool designed to manage context isolation and focused task execution with Claude Code.

## Key Overview

The tool addresses fundamental challenges when working with Claude on complex projects:

**Core Problem:** As stated in the documentation, "Claude has a limited context window. Long, complex projects can exceed this limit," and task switching can cause confusion about which work item is currently being addressed.

**Main Solution:** The "Boomerang" approach breaks large projects into smaller, self-contained tasks executed in isolated context windows, ensuring Claude maintains focus on individual tasks.

## Essential Features

The platform includes:
- Task breakdown and context isolation
- Real-time streaming output with modern Textual dashboard
- MCP (Model Context Protocol) integration
- Process pooling and performance optimization
- Demo mode for testing without API usage
- Comprehensive CLI and Python API

## Prerequisites & Setup

Users need Claude Desktop, the Claude Code CLI tool, and Desktop Commander installed. The installation uses `uv` for virtual environment management and includes commands like:

```bash
task-runner create <project_name> <task_list_file>
task-runner run [options]
```

The project is MIT-licensed and requires Python 3.10+.

---
Source: https://github.com/grahama1970/claude-task-runner
