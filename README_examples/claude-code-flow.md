# Claude-Flow v3: Enterprise AI Orchestration Platform

Claude-Flow v3 is a comprehensive multi-agent orchestration framework that transforms Claude Code into a production-ready platform for coordinating 54+ specialized AI agents. The system emphasizes self-learning capabilities, where agents improve over time through pattern recognition and adaptive routing.

## Core Architecture

The platform operates through several integrated layers:

- **Entry Layer**: CLI and MCP server interfaces with AIDefence security scanning
- **Routing Layer**: Q-Learning router with Mixture of Experts (8 specialized experts) and 42+ built-in skills
- **Swarm Coordination**: Supports multiple topologies (mesh, hierarchical, ring, star) with Byzantine fault tolerance
- **Agent Pool**: 54+ pre-built agents across development, security, testing, and DevOps domains
- **Intelligence Layer**: RuVector components including SONA (self-optimizing neural architecture), EWC++ memory consolidation, and HNSW vector search offering "150x-12,500x faster" retrieval

## Key Capabilities

**Self-Learning System**: The framework learns from execution outcomes through a four-step pipeline—retrieve similar patterns via HNSW, judge success/failure, distill learnings via LoRA compression, and consolidate knowledge using EWC++ to prevent catastrophic forgetting.

**Multi-Agent Coordination**: Queen-led hierarchical swarms with three queen types (Strategic, Tactical, Adaptive) coordinate eight worker specializations. Consensus mechanisms include Byzantine Fault Tolerance, Raft, Gossip, and CRDT protocols.

**Intelligent Model Routing**: Automatically routes tasks across six LLM providers—simple edits use WebAssembly (<1ms), medium complexity uses Haiku/Sonnet (~500ms), complex tasks use Opus. This approach claims to extend Claude Max usage by 250%.

**Memory & Embeddings**: AgentDB with HNSW indexing, multiple embedding providers (Agentic-Flow ONNX, OpenAI, Transformers.js), and persistent SQLite storage with LRU caching.

**Security Hardening**: Input validation via Zod schemas, path traversal prevention, command sandboxing, CVE monitoring, and AIDefence threat detection completing threat analysis in under 10ms.

## Installation & Setup

```bash
npm install claude-flow@v3alpha
npx claude-flow@v3alpha init
npx claude-flow@v3alpha mcp start
```

Integrates with Claude Desktop, VS Code, Cursor, Windsurf, and other MCP-compatible environments through standard configuration.

## Notable Features

- **27 Lifecycle Hooks**: Pre/post operations for file edits, commands, tasks, and sessions with automatic pattern learning
- **12 Background Workers**: Auto-triggered optimization, security audits, memory consolidation, and performance analysis
- **175+ MCP Tools**: Full tool registry with <10ms lookup times and support for resources, prompts, and async task management
- **42 Pre-Built Skills**: Reusable workflows covering GitHub integration, swarm orchestration, security hardening, and performance optimization
- **Spec-Driven Development**: Architecture Decision Records (ADRs) with Domain-Driven Design bounded contexts ensuring implementation compliance
- **Performance Targets**: 2.8-4.4x task speedup, 10-20x faster swarm spawning, 84.8% SWE-Bench solve rate

## Comparison to Alternatives

Unlike CrewAI, LangGraph, AutoGen, and other frameworks, Claude-Flow v3 uniquely combines self-learning (SONA + EWC++), 5 consensus protocols, Byzantine fault tolerance, and 42+ built-in skills. The platform emphasizes preventing knowledge loss through elastic weight consolidation while enabling 150x faster vector search through HNSW indexing.

The system represents a production-ready alternative for teams requiring enterprise-grade multi-agent coordination with continuous learning and fault tolerance across distributed workloads.

---
Source: https://github.com/ruvnet/claude-code-flow
