---
name: foundry-setup
description: First-time setup and guided tour for the claude-foundry plugin
---

# Foundry Setup Command

When invoked, execute these phases in order. This command is idempotent and safe to run multiple times.

## Phase 1: Pre-flight Checks

Run these checks in parallel to verify the environment:

1. Call `mcp__foundry-mcp__health-liveness` to verify MCP server connectivity
2. Call `mcp__foundry-mcp__sdd-verify-toolchain` to check required tools (Python, git)
3. Call `mcp__foundry-mcp__sdd-verify-environment` to validate runtime versions

Present results in a status table:

```
## Pre-flight Checks

| Check | Status | Details |
|-------|--------|---------|
| MCP Server | [✓/✗] | foundry-mcp responding / connection failed |
| Python | [✓/✗] | version number |
| Git | [✓/✗] | version number |
```

**If MCP server check fails:** Stop and explain that foundry-mcp must be configured. Point to installation instructions:
- PyPI: `pip install foundry-mcp`
- Or: `uvx foundry-mcp`
- Configure in Claude Code MCP settings

**If other checks fail:** Warn but continue - some features may not work.

## Phase 2: Workspace Setup

1. Call `mcp__foundry-mcp__sdd-detect-topology` to analyze the project structure
2. Call `mcp__foundry-mcp__spec-list` to check for existing specifications

### If specs/ directory does not exist:

Use `AskUserQuestion` to ask:
- Question: "Would you like to initialize the specs directory structure?"
- Options: "Yes, initialize" / "No, skip"

If yes: Call `mcp__foundry-mcp__sdd-init-workspace` to create:
- specs/active/
- specs/pending/
- specs/completed/
- specs/archived/

### If foundry-mcp.toml does not exist:

Use `AskUserQuestion` to ask:
- Question: "Would you like to create a foundry-mcp.toml configuration file?"
- Options: "Yes, create with defaults" / "No, skip"

If yes: Call `mcp__foundry-mcp__sdd-setup` with `dry_run=false`

Report what was created or skipped.

## Phase 3: Comprehensive Feature Tour

**Your role in this phase is to TEACH the user about SDD and the plugin.** Don't just dump information - explain concepts conversationally, connect ideas, and help the user understand *why* each piece matters.

### 3.1 Explain What SDD Is

Start by teaching the user about Spec-Driven Development:

> "Before we dive into the tools, let me explain what Spec-Driven Development (SDD) is and why it's powerful.
>
> **The Problem:** When building features, it's easy to jump straight into coding. But this often leads to scope creep, forgotten requirements, and difficulty tracking what's done.
>
> **The SDD Solution:** We create a detailed specification *before* writing any code. This spec becomes the single source of truth - it defines what we're building, breaks it into phases and tasks, and tracks our progress. Claude can then help you work through tasks systematically, verify your implementation matches the spec, and generate documentation automatically.
>
> Think of it like having a detailed blueprint before building a house."

Use `AskUserQuestion`:
- Question: "Make sense so far?"
- Options: "Yes, continue" / "I have a question"

If "I have a question": Pause and answer their question before continuing.

### 3.2 Walk Through the Workflow

Explain each stage of the workflow and how they connect:

```
    +------------------+
    |    sdd-plan      |  Create specification
    +--------+---------+
             |
             v
    +------------------+
    | sdd-plan-review  |  Review & refine
    +--------+---------+
             |
             v
    +------------------+
    |    sdd-next      |  Find next task
    +--------+---------+
             |
             v
    +------------------+
    |   [Implement]    |  Write code
    +--------+---------+
             |
             v
    +------------------+
    |   sdd-update     |  Track progress
    +--------+---------+
             |
             v
    +------------------+
    |    run-tests     |  Verify quality
    +--------+---------+
             |
             v
    +------------------+
    |     sdd-pr       |  Create PR
    +------------------+
```

For each stage, explain its purpose conversationally:

1. **Planning** - "This is where you describe what you want to build. The sdd-plan skill helps Claude create a structured spec with phases, tasks, and acceptance criteria."

2. **Review** - "Before implementing, it's worth having the spec reviewed. sdd-plan-review can consult multiple AI models to catch issues early."

3. **Implementation** - "Now you work through tasks one by one. sdd-next finds your next task and gives you all the context you need."

4. **Progress Tracking** - "As you complete tasks, sdd-update marks them done and journals your decisions. This creates an audit trail."

5. **Quality** - "run-tests and sdd-fidelity-review help verify your code matches the spec and passes tests."

6. **Delivery** - "When ready, sdd-pr creates a comprehensive PR description from your spec and journal entries."

Use `AskUserQuestion`:
- Question: "Ready to see the available skills?"
- Options: "Yes, show me" / "I have a question"

If "I have a question": Pause and answer their question before continuing.

### 3.3 Introduce the Skills

Now introduce each skill, grouped by when you'd use them. Explain not just *what* they do, but *when* and *why* you'd reach for them.

Skills are invoked with `Skill(skill-name)` or `Skill(skill-name) "context"`.

### Planning Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-plan** | Create detailed specifications before coding | `Skill(sdd-plan) "Add user authentication with OAuth"` |

### Implementation Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-next** | Find and prepare the next actionable task | `Skill(sdd-next)` |
| **doc-query** | Query codebase documentation for classes, functions, call graphs | `Skill(doc-query) "Find all authentication handlers"` |

### Progress Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-update** | Track progress, update task status, journal decisions | `Skill(sdd-update) "Mark task-1-2 as completed"` |

### Quality Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-validate** | Validate spec structure, auto-fix issues, analyze dependencies | `Skill(sdd-validate) "Check my-feature-spec"` |
| **run-tests** | Run pytest tests, debug failures, investigate errors | `Skill(run-tests) "Run tests for auth module"` |
| **sdd-fidelity-review** | Compare implementation against spec requirements | `Skill(sdd-fidelity-review) "Review phase-1 compliance"` |

### Review Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-plan-review** | Multi-model consultation for spec quality | `Skill(sdd-plan-review) "Review my-feature-spec"` |
| **sdd-modify** | Apply review feedback, bulk modifications to specs | `Skill(sdd-modify) "Apply suggestions from review"` |

### Delivery Stage

| Skill | Purpose | Example |
|-------|---------|---------|
| **sdd-pr** | Create AI-powered PR descriptions from spec context | `Skill(sdd-pr) "Create PR for my-feature-spec"` |
| **sdd-render** | Generate human-readable documentation from specs | `Skill(sdd-render) "Export my-feature-spec to markdown"` |

Use `AskUserQuestion`:
- Question: "Ready to see the available commands?"
- Options: "Yes, continue" / "I have a question"

If "I have a question": Pause and answer their question before continuing.

### 3.4 Commands

## Available Commands

| Command | Purpose |
|---------|---------|
| **/sdd-next** | Quick entry point to resume or start SDD work |
| **/foundry-setup** | This setup command (re-runnable) |

---

## Phase 4: Hands-On Learning with a Sample Spec

**The best way to learn SDD is by doing.** Offer to create a sample spec so the user can see the structure and try the workflow.

Use `AskUserQuestion` to ask:
- Question: "Would you like to create a sample 'hello-world' spec? This lets you explore the workflow hands-on without committing to a real feature yet."
- Options: "Yes, let's try it" / "No, I'll create my own when ready"

If yes:

1. Invoke `Skill(sdd-plan) "Create a hello-world demo spec for the foundry-setup tutorial. Requirements: A simple Python greeting utility with a greet(name='World') function that returns 'Hello, {name}!'. Include a CLI interface. Keep it simple - one implementation phase with 2 tasks."`

2. **TEACH the user about spec structure** by walking through what was created:

> "Great! I've created a sample spec. Let me walk you through its structure:
>
> **Location:** `specs/pending/hello-world-demo.json`
>
> Specs start in the `pending/` folder until you're ready to work on them. When you start, they move to `active/`, and when complete, to `completed/`.
>
> **Key sections in a spec:**
>
> - **metadata** - Title, version, status, and timestamps. This is the spec's identity.
>
> - **hierarchy** - The heart of the spec. Contains phases (major milestones) and tasks (individual work items). Each task has a status, description, and can have subtasks.
>
> - **assumptions** - Constraints and requirements that shape the implementation. Document these early to avoid surprises.
>
> - **journal** - A decision log that captures *why* you made choices, not just *what* you did. Invaluable for PRs and future reference.
>
> You interact with specs through MCP tools and skills - you don't edit the JSON directly. This keeps everything consistent and trackable."

3. Use `AskUserQuestion` to ask:
- Question: "Now that you've seen the structure, what would you like to do with this sample?"
- Options:
  - "Activate it and walk me through a task" - Best for hands-on learning
  - "Keep it in pending for later"
  - "Delete it - I understand the concept now"

4. Execute the chosen action:
   - If "activate and walk through": Call `mcp__foundry-mcp__spec-lifecycle-activate`, then invoke `Skill(sdd-next) "Sample spec activated via /foundry-setup tutorial. Walk the user through their first task to demonstrate the SDD workflow."`
   - If "keep in pending": Just confirm it's saved
   - If "delete": Remove it and confirm

5. After demonstrating task preparation, explain that this was a simplified introduction:

> "This tutorial showed the core workflow: `sdd-plan` to create specs and `sdd-next` to prepare tasks. The full SDD workflow also includes:
> - `sdd-plan-review` - Get AI feedback on your spec before implementing
> - `sdd-update` - Track progress and journal decisions as you work
> - `sdd-fidelity-review` - Verify your implementation matches the spec
> - `run-tests` - Run and debug tests
> - `sdd-pr` - Create a PR with full context from your spec and journal
>
> You can explore these as you get comfortable with the basics."

## Phase 5: Next Steps & Encouragement

**End on a helpful, encouraging note.** Summarize what was accomplished and give clear, actionable next steps based on their situation.

### Summarize the Setup

Recap what was configured:
- Environment status (what passed/failed)
- Workspace setup (what was created)
- Sample spec status (if created)

### Personalized Next Steps

Based on the user's state, recommend the most logical next action:

**If they created and activated a sample spec:**
> "You're all set! Your sample spec is active and ready. Try asking me to run `sdd-next` - I'll show you how task preparation works and walk you through completing your first task."

**If they have existing specs:**
> "You're all set! I see you already have specs in your workspace. Run `/sdd-next` anytime to pick up where you left off, or ask me to create a new spec with `sdd-plan`."

**If they have a clean workspace:**
> "You're all set! When you're ready to start a feature, just describe what you want to build and ask me to use `sdd-plan`. I'll create a detailed spec and we can work through it together."

### Resources for Learning More

> "A few resources if you want to go deeper:
>
> - **Plugin docs:** `docs/claude_code_best_practices/` - How to build Claude Code plugins
> - **Foundry reference:** `docs/foundry_mcp_reference/` - Deep dive into MCP tools and workflows
> - **Re-run setup:** `/foundry-setup` is safe to run anytime if you need a refresher
>
> Feel free to ask me questions anytime - I'm here to help you get the most out of SDD!"

### Close Warmly

End with something like:
> "That's the tour! The key thing to remember: specs first, then code. It takes a bit of discipline upfront but pays off massively in clarity, trackability, and documentation. Ready when you are!"
