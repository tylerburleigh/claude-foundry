---
name: sdd-refactor
description: Safe refactoring operations using LSP for precise symbol resolution. Supports rename, extract function/method, and move symbol operations with automatic reference updates.
---

# SDD-Refactor: Safe Refactoring Skill

## Overview

`Skill(foundry:sdd-refactor)` provides LSP-powered refactoring with safety checks. It uses `findReferences` to understand impact before making changes, performs batch edits, and verifies correctness.

**Core capabilities:**
- Safe rename operations with complete reference updates
- Extract function/method with reference updating
- Move symbol across files with import fixups
- Unused code cleanup based on zero-reference analysis

**IMPORTANT:** This skill uses Claude Code's built-in LSP tools for precise refactoring. If LSP returns no results for the target language, fall back to Grep-based patterns.

## Skill Family

This skill integrates with the **Spec-Driven Development** workflow:

```
sdd-plan --> sdd-refactor (for planned refactoring tasks) --> sdd-update --> sdd-fidelity-review
```

Can also be used standalone for ad-hoc refactoring outside of specs.

## When to Use This Skill

Use `Skill(foundry:sdd-refactor)` for:
- Renaming classes, functions, methods, or variables across codebase
- Extracting code into new functions/methods
- Moving symbols between files/modules
- Cleaning up unused code (dead code removal)
- Any refactoring that affects multiple files

**Do NOT use for:**
- Simple single-file edits (use Edit tool directly)
- Creating new code from scratch (use sdd-plan + implement)
- Deleting entire files (manual operation)
- Formatting or style changes (use formatter tools)

## LSP Tools

This skill uses Claude Code's built-in LSP tools directly:

| Tool | Purpose |
|------|---------|
| `findReferences` | Find all usages of a symbol |
| `goToDefinition` | Navigate to symbol definition |
| `documentSymbol` | Get file structure/symbol outline |

For spec integration, use foundry-mcp:

| Router | Actions | Purpose |
|--------|---------|---------|
| `task` | `update-status`, `complete` | Track refactoring task progress |
| `journal` | `add` | Document refactoring decisions |

## Core Workflow

### Step 1: Identify Refactoring Target

Gather information about what to refactor:

1. **User provides:**
   - Symbol name (class, function, variable)
   - File path containing the symbol
   - Refactoring type (rename, extract, move, cleanup)
   - New name or destination (if applicable)

2. **Validate target exists with LSP:**
   ```
   definition = goToDefinition(file="src/module.py", symbol="OldClassName", line=10)

   if definition found:
       Proceed to impact analysis
   else:
       Ask user to verify symbol name and location
   ```

3. **Fallback if LSP unavailable:**
   ```
   Use Grep to verify symbol exists:
   Grep(pattern="class OldClassName", path="src/")
   ```

### Step 2: Impact Analysis (REQUIRED)

**NEVER refactor without understanding impact first.**

#### 2.1 LSP-Enhanced Impact Analysis

```
# Get all references to the symbol
references = findReferences(file="src/module.py", symbol="OldClassName", line=10, character=6)

# Analyze results:
- Total reference count
- Unique files affected
- Reference types (import, call, type annotation, assignment)
- Test files vs production files
```

#### 2.2 Impact Report

Present to user before proceeding:

```markdown
## Refactoring Impact: Rename OldClassName -> NewClassName

**Total References:** 47 across 12 files

**By Category:**
- Imports: 12 files
- Class instantiation: 8 locations
- Type annotations: 15 locations
- Inheritance: 2 locations

**Files Affected:**
- src/auth/service.py (8 refs)
- src/api/handlers.py (12 refs)
- tests/test_auth.py (15 refs)
- ... (9 more files)

**Risk Assessment:** MEDIUM
- All references in controlled codebase
- Good test coverage (15 test refs)

Proceed? [Execute] [Dry Run] [Cancel]
```

#### 2.3 Fallback Impact Analysis

If LSP unavailable:

```
Use Grep to find references:
Grep(pattern="OldClassName", path="src/", output_mode="content")

WARNING: Grep-based analysis may include false positives (comments, strings).
Review each match before proceeding.
```

### Step 3: Execute Refactoring

#### 3.1 Rename Operation

**With LSP:**
1. Collect all reference locations from `findReferences`
2. Sort files by dependency order (imports first, then usages)
3. For each file:
   - Read file content
   - Replace symbol at LSP-provided positions (precise, not text search)
   - Write updated file
4. Verify rename succeeded:
   ```
   findReferences(file="src/module.py", symbol="NewClassName")
   # Should return same count as before
   ```

**Without LSP (Manual):**
1. Use Grep to find all occurrences
2. Review each match for false positives
3. Edit files one at a time
4. Verify with Grep that old name no longer appears

#### 3.2 Extract Function/Method

1. **Identify code block** to extract (line range)
2. **Analyze variables:**
   - Inputs: referenced but not defined in block
   - Outputs: defined in block, used after
3. **Create new function** with appropriate signature
4. **Replace original code** with function call
5. **Verify structure:**
   ```
   documentSymbol(file="src/module.py")
   # Confirm new function appears in symbols
   ```

#### 3.3 Move Symbol

1. **Find all references** to the symbol
2. **Move definition** to new file
3. **Add export** in new location (if needed)
4. **Update all imports** across codebase
5. **Remove from original** file
6. **Verify no broken references:**
   ```
   findReferences(file="new/location.py", symbol="MovedSymbol")
   # All references should resolve
   ```

#### 3.4 Dead Code Cleanup

1. **Identify candidate** symbols for removal
2. **Check reference count:**
   ```
   references = findReferences(file="src/utils.py", symbol="unused_function")

   if references.count == 0:
       Safe to remove
   elif references.count == 1 and reference is definition:
       Safe to remove (only self-reference)
   else:
       NOT safe - has usages
   ```
3. **Remove symbol** if safe
4. **Clean up imports** that referenced removed symbol

### Step 4: Verify Correctness

After any refactoring:

1. **Structural verification (LSP):**
   ```
   documentSymbol(file="src/module.py")
   # Verify expected structure preserved
   ```

2. **Reference verification (LSP):**
   ```
   findReferences(file="src/module.py", symbol="NewName")
   # Verify all references resolve
   ```

3. **Run affected tests:**
   ```
   Skill(foundry:run-tests) "Run tests for affected files"
   ```

### Step 5: Document Changes

If refactoring is part of a spec task:

```bash
mcp__plugin_foundry_foundry-mcp__task action="complete" spec_id="{spec-id}" task_id="{task-id}" completion_notes="Renamed OldClassName to NewClassName across 12 files. All tests passing."
```

For significant refactors, add journal entry:

```bash
mcp__plugin_foundry_foundry-mcp__journal action="add" spec_id="{spec-id}" title="Refactoring: OldClassName -> NewClassName" content="Renamed class for clarity. 47 references updated across 12 files. No functional changes." entry_type="decision"
```

## LSP Availability Check

Before using LSP-enhanced workflow, verify availability:

```
# Try to get symbols from target file
symbols = documentSymbol(file="target_file.py")

if symbols returned successfully:
    Use LSP-enhanced workflow
else:
    Fall back to Grep-based workflow
```

**Fallback triggers:**
- LSP tool returns error
- No language server for file type (e.g., Makefile, .txt)
- Empty result for known non-empty file
- Timeout (>5 seconds)

## Size Guidelines

| Scope | Approach |
|-------|----------|
| Single file, <10 refs | Direct refactoring |
| Cross-file, <50 refs | Batch with verification |
| 50-100 refs | Split into incremental changes |
| >100 refs | Create spec for tracking, phase the work |

## Example Invocations

**Rename class:**
```
Skill(foundry:sdd-refactor) "Rename AuthService to AuthenticationService in src/auth/service.py"
```

**Extract method:**
```
Skill(foundry:sdd-refactor) "Extract lines 45-67 in src/handlers.py into new method validate_request"
```

**Move function:**
```
Skill(foundry:sdd-refactor) "Move helper_function from src/utils.py to src/helpers/common.py"
```

**Dead code cleanup:**
```
Skill(foundry:sdd-refactor) "Find and remove unused functions in src/legacy/"
```

**Rename with spec context:**
```
Skill(foundry:sdd-refactor) "Complete task-2-3: Rename UserDTO to UserResponse as specified"
```
