# Implementation Plan: Programmatic MCP Toggle

## Summary

Create `foundry-mcp-ctl` package (co-located in foundry-mcp repo) that enables programmatic toggling of SDD tools to save context.

**Key decisions:**
- Package lives in foundry-mcp repo
- Default mode: minimal (1 "wake" tool)
- Full mode: all 17 routers
- Restart latency (~1-2s) is acceptable

---

## Architecture

```
┌─────────────────┐     ┌─────────────────┐
│  Claude Code    │     │  Claude Code    │
└────────┬────────┘     └────────┬────────┘
         │                       │
         │ stdio                 │ stdio
         │                       │
┌────────▼────────┐     ┌────────▼────────┐
│  foundry-mcp    │     │  foundry-ctl    │
│  (via wrapper)  │     │  (helper MCP)   │
└────────┬────────┘     └─────────────────┘
         │                       │
         │              writes signal file
         │                       │
┌────────▼────────┐              │
│    wrapper      │◄─────────────┘
│  (monitors for  │
│   restart sig)  │
└─────────────────┘
```

---

## Phase 1: Create foundry-mcp-ctl package

**Location:** `foundry-mcp/src/foundry_mcp_ctl/`

### Files to create:

**`__init__.py`**
```python
__version__ = "0.1.0"
```

**`__main__.py`** (~50 lines)
- CLI entry point with Click
- Commands: `wrap`, `helper`, `restart`, `clean`, `set-mode`

**`wrapper.py`** (~100 lines)
- Launch foundry-mcp as subprocess
- Pass stdin/stdout through
- Monitor `~/.foundry-mcp-ctl/{server}.restart` for signals
- On signal: read mode from config, restart with FOUNDRY_MODE env var
- Graceful shutdown (SIGTERM, then SIGKILL after 5s)
- Clear `__pycache__` on restart

**`helper.py`** (~80 lines)
- FastMCP server with 1 tool: `set_mode`
- `set_mode(mode: str)`: writes mode + restart signal
- Returns status to Claude

**`config.py`** (~30 lines)
- Read/write mode to `~/.foundry-mcp-ctl/config.json`
- Default mode: "minimal"

---

## Phase 2: Modify foundry-mcp for mode support

**File:** `foundry-mcp/src/foundry_mcp/config.py`

Add:
```python
def get_mode() -> str:
    return os.environ.get("FOUNDRY_MODE", "minimal")
```

**File:** `foundry-mcp/src/foundry_mcp/tools/unified/__init__.py`

Modify `register_unified_tools()`:
```python
def register_unified_tools(mcp: FastMCP, config: ServerConfig) -> None:
    mode = get_mode()

    if mode == "minimal":
        register_wake_tool(mcp, config)  # Just 1 tool
    else:
        # Register all 17 routers (existing code)
        register_unified_health_tool(mcp, config)
        register_unified_plan_tool(mcp, config)
        # ... etc
```

**File:** `foundry-mcp/src/foundry_mcp/tools/unified/wake.py` (NEW)

```python
def register_wake_tool(mcp: FastMCP, config: ServerConfig) -> None:
    @mcp.tool()
    def wake() -> dict:
        """
        SDD tools are in minimal mode. To enable full SDD:
        1. Run /sdd-on command, OR
        2. Use @foundry-ctl set_mode mode="full"
        """
        return {
            "status": "minimal_mode",
            "message": "Run /sdd-on to enable full SDD tools"
        }
```

---

## Phase 3: Create claude-foundry commands

**File:** `claude-foundry/commands/sdd-on.md`
```markdown
# /sdd-on

Enable full SDD tools.

## Steps
1. Call: mcp__foundry-ctl__set_mode mode="full"
2. Wait for restart (~1-2 seconds)
3. Confirm: "SDD tools enabled. All 17 routers available."
```

**File:** `claude-foundry/commands/sdd-off.md`
```markdown
# /sdd-off

Disable SDD tools to save context.

## Steps
1. Call: mcp__foundry-ctl__set_mode mode="minimal"
2. Wait for restart (~1-2 seconds)
3. Confirm: "SDD tools disabled. ~3,300 tokens freed."
```

---

## Phase 4: Update MCP configuration

**User's `.mcp.json` becomes:**
```json
{
  "mcpServers": {
    "foundry-mcp": {
      "command": "python",
      "args": ["-m", "foundry_mcp_ctl", "wrap", "--name", "foundry-mcp", "--", "python", "-m", "foundry_mcp.server"]
    },
    "foundry-ctl": {
      "command": "python",
      "args": ["-m", "foundry_mcp_ctl", "helper"]
    }
  }
}
```

**Update foundry-setup command** to generate this config.

---

## Phase 5: Update pyproject.toml

**File:** `foundry-mcp/pyproject.toml`

Add entry point:
```toml
[project.scripts]
foundry-mcp = "foundry_mcp.server:main"
foundry-mcp-ctl = "foundry_mcp_ctl.__main__:main"
```

---

## File Summary

### New files (in foundry-mcp repo):
- `src/foundry_mcp_ctl/__init__.py`
- `src/foundry_mcp_ctl/__main__.py`
- `src/foundry_mcp_ctl/wrapper.py`
- `src/foundry_mcp_ctl/helper.py`
- `src/foundry_mcp_ctl/config.py`
- `src/foundry_mcp/tools/unified/wake.py`

### Modified files (in foundry-mcp repo):
- `src/foundry_mcp/config.py` - add get_mode()
- `src/foundry_mcp/tools/unified/__init__.py` - conditional registration
- `pyproject.toml` - add entry point

### New files (in claude-foundry repo):
- `commands/sdd-on.md`
- `commands/sdd-off.md`

### Modified files (in claude-foundry repo):
- `commands/foundry-setup.md` - update MCP config instructions

---

## Testing Plan

1. **Unit tests** for wrapper signal handling
2. **Integration test**: `/sdd-off` → verify 1 tool → `/sdd-on` → verify 17 tools
3. **Context measurement**: Compare token usage before/after

---

## Rollout

1. Implement in foundry-mcp repo first
2. Test locally with manual MCP config
3. Update claude-foundry commands
4. Update setup instructions
5. Document in README
