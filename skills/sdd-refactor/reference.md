# SDD-Refactor Reference

Detailed workflows and procedures for the sdd-refactor skill.

## Table of Contents

- [Impact Analysis](#impact-analysis)
- [Refactoring Operations](#refactoring-operations)
- [LSP Availability Check](#lsp-availability-check)
- [Troubleshooting](#troubleshooting)

---

## Impact Analysis

**NEVER refactor without understanding impact first.**

### LSP-Enhanced Impact Analysis

```
# Get all references to the symbol
references = findReferences(file="src/module.py", symbol="OldClassName", line=10, character=6)

# Analyze results:
- Total reference count
- Unique files affected
- Reference types (import, call, type annotation, assignment)
- Test files vs production files
```

### Impact Report Format

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

### Fallback Impact Analysis

If LSP unavailable:

```
Use Grep to find references:
Grep(pattern="OldClassName", path="src/", output_mode="content")

WARNING: Grep-based analysis may include false positives (comments, strings).
Review each match before proceeding.
```

---

## Refactoring Operations

### Rename Operation

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

### Extract Function/Method

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

### Move Symbol

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

### Dead Code Cleanup

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

---

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

---

## Troubleshooting

### LSP Returns No Results

**Symptoms:** `findReferences` or `documentSymbol` returns empty

**Checks:**
1. Verify file type has LSP support (Python, TypeScript, Go, etc.)
2. Ensure language server is running
3. Check that symbol exists at specified line/character position

**Resolution:** Fall back to Grep-based workflow

### Rename Breaks Imports

**Symptoms:** After rename, imports fail to resolve

**Checks:**
1. Verify all import statements were updated
2. Check for dynamic imports or string-based references
3. Look for re-exports that may have been missed

**Resolution:**
1. Use `Grep(pattern="OldName")` to find remaining references
2. Manually update any missed locations

### Extract Creates Invalid Code

**Symptoms:** Extracted function has wrong signature or missing variables

**Checks:**
1. Verify all input variables identified
2. Check for closures or captured variables
3. Ensure return values properly handled

**Resolution:** Manually adjust function signature and call site

### Dead Code Detection False Positives

**Symptoms:** LSP says zero references, but code is used

**Causes:**
- Dynamic references (`getattr`, reflection)
- String-based lookups
- External entry points (CLI, API endpoints)
- Test-only code

**Resolution:** Check for dynamic usage patterns before removal
