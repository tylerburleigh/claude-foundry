# Git Commit Integration

Automatic commit workflow when git integration is enabled.

When git integration is enabled via the `[git]` block in `foundry-mcp.toml` (or the corresponding `FOUNDRY_MCP_GIT_*` environment variables), the sdd-update skill can automatically create commits after task completion based on the configured commit cadence.

## Commit Cadence Configuration

The commit cadence determines when to offer automatic commits:

- **task**: Commit after each task completion (frequent commits, granular history)
- **phase**: Commit after each phase completion (fewer commits, milestone-based)
- **manual**: Never auto-commit (user manages commits manually)

Set `commit_cadence` inside the `[git]` section of `foundry-mcp.toml` (or expose it via `FOUNDRY_MCP_GIT_COMMIT_CADENCE`). The MCP server shares that value across all connected clients so they make the same commit decisions.

---

## Commit Workflow Steps

When completing a task and git integration is enabled, the workflow follows these steps:

### 1. Check Commit Cadence

First, confirm whether automatic commits are enabled for the current event type. `sdd-update` reads the MCP configuration (`foundry-mcp.toml` → `[git]`) at runtime, so you only need to inspect that file (or use your preferred helper command) to see whether the cadence is set to `task`, `phase`, or `manual`.

### 2. Check for Changes

Before offering a commit, verify there are uncommitted changes:

```bash
# Check for changes (run in repo root directory)
git status --porcelain

# If output is empty, skip commit offer (nothing to commit)
# If output has content, proceed with commit workflow
```

### 3. Generate Commit Message

Create a structured commit message from task metadata using the pattern `{task-id}: {task-title}` (for example, `task-2-3: Implement JWT verification middleware`). The MCP helper applies this format automatically when generating commits.

### 4. Preview and Stage Changes (Two-Step Workflow)

The workflow now supports **agent-controlled file staging** with two approaches:

**Option A: Show Preview (Default - Recommended)**

When `file_staging.show_before_commit = true` (default), the agent sees uncommitted files and can selectively stage:

```bash
# Step 1: Preview uncommitted files (automatic via show_commit_preview_and_wait)
# Shows: modified, untracked, and staged files
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id}

# Step 2: Agent stages only task-related files
git add specs/active/spec.json
git add src/feature/implementation.py
git add tests/test_feature.py
# (Deliberately skip unrelated files like debug scripts, personal notes)

# Step 3: Create commit with staged files only (via git directly after staging)
git commit -m "{task-id}: {task-title}"
```

**Benefits:**
- Agent controls what files are committed
- Unrelated files protected from accidental commits
- Clean, focused task commits

**Option B: Auto-Stage All (Backward Compatible)**

When `file_staging.show_before_commit = false`, the old behavior is preserved:

```bash
# Automatically stages all files and commits (old behavior)
git add --all
git commit -m "{task-id}: {task-title}"
```

**Configuration:**

File staging behavior is controlled in `foundry-mcp.toml`:

```toml
[git]
enabled = true
auto_commit = true
commit_cadence = "task"
show_before_commit = true  # false = auto-stage all (backward compatible)
```

**Command Reference:**

```bash
# Complete task (shows preview if enabled)
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id}

# Example workflow:
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id="user-auth-001" task_id="task-1-2"
# (Review preview, stage desired files)
git add specs/active/user-auth-001.json src/auth/service.py
git commit -m "task-1-2: Implement JWT verification middleware"
```

**All git commands use `cwd=repo_root`** obtained from `find_git_root()` to ensure they run in the correct repository directory.

### 5. Record Commit Metadata

After successful commit, capture the commit SHA and update spec metadata:

```bash
# Get the commit SHA
git rev-parse HEAD

# Example output: a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0
```

---

## Error Handling

Git operations should be non-blocking - failures should not prevent task completion:

**Error handling principles:**

- Check `returncode` for all git commands
- Log warnings for failures but continue execution
- Git failures do NOT prevent task completion
- User can manually commit if automatic commit fails
- Provide clear error messages in logs for debugging

---

## Repository Root Detection

All git commands must run in the repository root directory:

- `sdd-update` automatically discovers the repo root (similar to running `git rev-parse --show-toplevel` from the spec directory).
- If no git repository is detected, the commit workflow is skipped and the task still completes.
- Manual workflows should `cd` to the project root (the directory containing `.git/`) before running git commands.

---

## Complete Example Workflow

1. Complete the task with `mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id={spec-id} task_id={task-id}`.
2. If git integration is enabled and cadence matches the event, `sdd-update` checks for uncommitted changes.
3. Stage files either via the preview workflow (recommended) or `git add --all` when auto-staging is enabled.
4. Create the commit with `git commit -m "{task-id}: {task-title}"`.
5. If no changes are staged or the git command fails, the tool logs a warning and the task completion still succeeds.

---

## Git Configuration File

Git integration is configured via the `[git]` section in `foundry-mcp.toml` (or the `FOUNDRY_MCP_GIT_*` environment variables):

```toml
[git]
enabled = true
auto_branch = true
auto_commit = true
auto_push = false
auto_pr = false
commit_cadence = "task"
```

**Settings:**
- `enabled`: Enable/disable all git integration features
- `auto_commit`: Offer automatic commits when the cadence matches the current event
- `commit_cadence`: When to commit - `"task"`, `"phase"`, or `"manual"`
