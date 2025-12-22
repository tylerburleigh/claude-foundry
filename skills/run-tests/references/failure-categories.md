# Failure Categories

## Assertion Failures

Expected vs actual mismatch. Investigate:

- **Test expectations correct?** - Has the expected behavior changed?
- **Implementation changed?** - Did recent changes alter output?
- **Data setup correct?** - Is test data/fixtures properly configured?

**Common causes:**
- API response format changed
- Floating point precision issues
- String encoding differences
- Order-dependent comparisons (sets, dicts)

## Exception Failures

Runtime errors during test execution:

| Error Type | Common Cause | Fix |
|------------|--------------|-----|
| `AttributeError` | Object missing attribute | Check object initialization |
| `KeyError` | Dictionary key missing | Validate data structure |
| `TypeError` | Wrong argument types | Check function signatures |
| `ValueError` | Invalid value passed | Validate input data |
| `IndexError` | List index out of range | Check collection sizes |

## Import Failures

Module loading issues:

**Check these in order:**
1. PYTHONPATH correct? (`export PYTHONPATH=src:$PYTHONPATH`)
2. `__init__.py` files exist in all package directories?
3. Circular imports? (A imports B, B imports A)
4. Missing dependencies? (`pip install -r requirements.txt`)

**Diagnosing circular imports:**
```python
# In the test file, add at the top:
import sys
print([m for m in sys.modules if 'your_module' in m])
```

## Fixture Failures

Setup/teardown issues:

| Issue | Check |
|-------|-------|
| Fixture not found | Is it in conftest.py or same file? |
| Fixture name wrong | Does the parameter name match exactly? |
| Fixture scope issue | Is scope (function/class/module/session) appropriate? |
| Fixture dependency | Does fixture depend on another fixture that fails? |

**Fixture scopes:**
- `function` - New instance per test (default)
- `class` - Shared across class
- `module` - Shared across module
- `session` - Shared across entire test session

## Timeout Failures

Tests hanging or taking too long:

**Investigation steps:**
1. Run test in isolation to confirm it's slow
2. Add debug logging to identify bottleneck
3. Check for infinite loops or blocking I/O
4. Look for missing mocks of external services

**Setting test timeouts:**
```python
@pytest.mark.timeout(30)  # 30 second limit
def test_slow_operation():
    pass
```

## Flaky Tests

Non-deterministic failures:

**Common causes:**
- Race conditions in async code
- Time-dependent logic (dates, timestamps)
- Random data generation without seeding
- Shared state between tests
- External service dependencies

**Strategies:**
- Run test multiple times: `pytest --count=10 test_file.py::test_name`
- Seed random generators: `random.seed(42)`
- Mock time-dependent functions
- Use pytest-randomly to detect order dependencies
