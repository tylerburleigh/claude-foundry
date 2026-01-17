# Task Master README Summary

**Task Master** is an AI-driven task management system designed for development workflows with Claude, Cursor AI, and other editors. Created by @eyaltoledano and @RalphEcom, it enables seamless project organization through intelligent task parsing and management.

## Key Features

The system supports multiple installation methods, with MCP (Model Control Protocol) being the recommended approach for editor integration. It works across Cursor, Windsurf, VS Code, and Q Developer CLI.

Task Master requires at least one API key from providers like Anthropic, OpenAI, Google Gemini, Perplexity, xAI, or OpenRouter. "Using the research model is optional but highly recommended."

## Setup & Configuration

Users can configure selective tool loading through the `TASK_MASTER_TOOLS` environment variable, choosing between:
- **All** (36 tools, ~21,000 tokens)
- **Standard** (15 tools, ~10,000 tokens)
- **Core** (7 tools, ~5,000 tokens)
- **Custom** (specific tools listed)

After initialization, the workflow involves creating a PRD (Product Requirements Document), parsing requirements, and managing tasks through commands like `parse-prd`, `next`, and `show`.

## Licensing

Task Master operates under MIT License with Commons Clause. Users can "Use Task Master for any purpose (personal, commercial, academic)" and modify/distribute copies, but cannot sell the tool itself or "Offer Task Master as a hosted service."

---
Source: https://github.com/eyaltoledano/claude-task-master
