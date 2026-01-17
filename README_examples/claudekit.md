# claudekit - Complete README

claudekit is a comprehensive toolkit for Claude Code that provides smart guardrails and workflow automation. Here's what it offers:

## Core Features

**Safety & Prevention**
- Real-time error detection blocking risky patterns (195+ sensitive file protections, TypeScript `any` type restrictions)
- Advanced bash security analysis for shell command vulnerabilities
- Git checkpoint system for easy rollback of failed changes

**Development Workflow**
- Instant codebase navigation so Claude understands project structure automatically
- Multi-aspect code reviews using 6 parallel specialized agents
- Iterative specification implementation with 6-phase quality workflows
- Smart git commands that follow project conventions

**AI Enhancement**
- Specialized subagents for domains (TypeScript, React, databases, testing, infrastructure)
- Configurable thinking levels to enhance Claude's reasoning
- Research expert agent for parallel information gathering (90% faster)
- Custom subagent creation

**Performance & Monitoring**
- Hook profiling to identify slow validations
- Session-based hook control for temporary adjustments
- Multi-tool ignore file support (respects `.agentignore`, `.aiignore`, `.cursorignore` and others)

## Installation & Setup

Requires Claude Code Max plan and Node.js 20+:
```bash
npm install -g claudekit
claudekit setup
```

## Key Commands

"Smart guardrails and workflow automation for Claude Code - catch errors in real-time, save checkpoints, and enhance AI coding with expert subagents" summarizes the tool's purpose.

Popular commands include `/code-review`, `/spec:create`, `/checkpoint:restore`, `/research`, and `/git:status`.

The project is MIT-licensed and actively maintained with comprehensive documentation, troubleshooting guides, and integration examples.

---
Source: https://github.com/carlrannaberg/claudekit
