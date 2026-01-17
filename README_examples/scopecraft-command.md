# Scopecraft Command - Complete README

A powerful CLI tool and MCP server for managing Markdown-Driven Task Management (MDTM) files. Scopecraft organizes tasks, features, and development workflows with structured approaches including two-state workflow, phase-based organization, and parent tasks with subtask sequencing.

## Key Capabilities

The platform supports "two-state workflow with phase-based task organization (current/archive + phase metadata)" and works across multiple AI IDEs through flexible project root configuration. Features include parent task management, subtask sequencing, both CLI and MCP server interfaces, and specialized Claude commands for development.

## Installation Methods

**Global Installation:**
```bash
npm install -g @scopecraft/cmd
yarn global add @scopecraft/cmd
bun install -g @scopecraft/cmd
```

**Via npx (no installation required):**
```bash
npx @scopecraft/cmd sc task list
npx --package=@scopecraft/cmd scopecraft-stdio --root-dir /path/to/project
```

**From source:**
```bash
git clone https://github.com/scopecraft-ai/scopecraft-command.git
cd scopecraft-command
bun install && bun run build && bun run install:global
```

## Core Workflows

Task management follows entity-command patterns (`task`, `parent`, `env`, `work`, `dispatch`). Common operations include task creation, status updates, parent task organization with subtasks, and interactive or autonomous development modes. The platform includes specialized Claude commands for feature ideation, proposal creation, PRD expansion, and implementation guidance across multiple technology domains.

## Integration

Scopecraft functions as standalone MDTM file management or integrates with Roo Commander via MCP server. Project type detection is automatic, requiring no special configuration. The MIT-licensed tool implements the MDTM format standardized by Roo Commander.

---
Source: https://github.com/scopecraft/command
