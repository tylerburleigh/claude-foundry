# claude-hooks README

`claude-hooks` is a TypeScript-based hook system for Claude Code that provides "full type safety, auto-completion, and access to strongly-typed payloads."

## Key Features

The tool enables developers to customize Claude Code's behavior through hooks with complete TypeScript support. Users can "write hooks with full type safety and IntelliSense" and access "strongly-typed payload data for all hook types (PreToolUse, PostToolUse, Notification, Stop)."

## Setup Requirements

The system requires Bun runtime for executing hooks and Node.js 18+ for running the CLI. Installation is straightforward: running `npx claude-hooks` initializes the development environment automatically.

## What Gets Created

The CLI generates a structured directory containing:
- Configuration file (`.claude/settings.json`)
- Main hook handlers in TypeScript (`.claude/hooks/index.ts`)
- Type definitions and utilities
- Optional session tracking functionality

Session data persists to the system's temporary directory.

## Development Experience

Developers write regular TypeScript with full IDE support. The generated code includes examples demonstrating typed payload access and custom logic integration, allowing use of npm packages and modern async patterns.

Installation options include both global setup (`npm install -g`) and per-project usage via npx.

---
Source: https://github.com/johnlindquist/claude-hooks
