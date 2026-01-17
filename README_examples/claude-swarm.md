# SwarmSDK, SwarmCLI & SwarmMemory - Complete README

## Overview

This is a Ruby framework for orchestrating multiple AI agents as a collaborative team with persistent memory. SwarmSDK represents a complete redesign of Claude Swarm, offering improved developer experience and support for general-purpose agentic systems.

## Key Capabilities

The framework delivers several significant advantages:

- **"Decoupled from Claude Code"** - eliminates the earlier dependency
- **"Single Process Architecture"** - all agents operate within one Ruby process using RubyLLM
- **"Direct method calls instead of MCP inter-process communication"** - more efficient than previous versions
- **"Node workflows, hooks system, scratchpad/memory tools, and more"** - expanded feature set
- **"Multiple LLM Providers"** - supports Claude, OpenAI, Gemini, and others through RubyLLM

## Getting Started

Installation is straightforward:

```bash
gem install swarm_cli     # Includes swarm_sdk
swarm --help
```

Configuration uses YAML format with agents, roles, tools, and delegation patterns. The framework supports both interactive REPL mode and command-line execution with specific prompts.

## Documentation Structure

Comprehensive guides cover:

- Fundamentals and core concepts
- **"Complete tutorial covering every feature"** across eight parts
- Architecture and execution flow diagrams
- CLI and Ruby DSL references
- YAML configuration specifications
- Integration examples (Rails, plugins, custom adapters)

## Advanced Features

**Node Workflows** enable multi-stage processing pipelines with dependencies between stages. The **Hooks System** provides custom logic execution at 12 different events. **SwarmMemory** adds persistent knowledge storage with semantic search capabilities using FAISS indexing.

## Comparison with v1

The framework offers significant improvements over Claude Swarm v1, including built-in memory systems, plugin architecture, node workflows, and cost tracking—while maintaining support for v1 for users preferring the multi-process approach.

---
Source: https://github.com/parruda/claude-swarm
