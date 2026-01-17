# Claude Squad README

Claude Squad is a terminal application that orchestrates multiple AI coding assistants (Claude Code, Codex, Gemini, and Aider) across isolated workspaces, enabling parallel task execution.

## Key Features

The application provides "complete tasks in the background (including yolo / auto-accept mode!)" and allows users to "manage instances and tasks in one terminal window." Each task operates in its own git workspace to prevent conflicts.

## Installation Options

Users can install via Homebrew or a manual script that places the binary in `~/.local/bin`. The tool requires tmux and the GitHub CLI as prerequisites.

## Core Functionality

The interface supports session management (creating, deleting, navigating between instances), task actions (attaching to sessions, committing changes, pausing/resuming), and basic navigation. The application uses three underlying technologies: tmux for isolated sessions, git worktrees for separate codebases, and a text-based UI for management.

## Configuration

The default program is Claude, with support for other assistants via command flags like `-p "aider ..."` or environment variables for API keys. Users can customize defaults through a config file.

The project is licensed under AGPL-3.0 and includes CI/CD automation via GitHub Actions.

---
Source: https://github.com/smtg-ai/claude-squad
