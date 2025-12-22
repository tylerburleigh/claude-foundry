# Advanced Troubleshooting

## Test Collection Errors

If pytest can't even collect tests:

```bash
pytest --collect-only tests/  # See what pytest finds
```

**Common causes:**
- Syntax errors in test files
- Import errors in test files
- Invalid test naming (`test_` prefix required)

## Coverage Gaps

Find untested code:

```bash
pytest --cov=src --cov-report=term-missing
```

## Parallel Test Execution

Run tests in parallel for speed:

```bash
pytest -n auto  # Uses pytest-xdist
```

**Note:** Parallel execution requires tests to be independent. If tests fail only in parallel, they likely have shared state issues.

## Test Isolation Verification

Ensure tests don't affect each other:

```bash
pytest --forked  # Run each test in subprocess
```

---

## Success Criteria

A test session is successful when:
- All tests pass
- No regressions introduced
- Root cause understood
- Fix documented (if complex)

**Post-fix checklist:**
1. Run the specific failing test
2. Run related tests in the same module
3. Run full test suite
4. Verify no new failures introduced
