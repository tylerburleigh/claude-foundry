# Common Fixes

## Pattern: Missing Mock

**Symptom:** Test makes real network calls, causing timeouts or failures

**Fix:**
```python
from unittest.mock import patch

@patch('module.requests.get')
def test_api_call(mock_get):
    mock_get.return_value.json.return_value = {'key': 'value'}
    result = function_under_test()
    assert result == expected
```

## Pattern: Fixture Scope Mismatch

**Symptom:** Test state leaking between tests

**Fix:** Use appropriate fixture scope or add cleanup
```python
@pytest.fixture(scope='function')  # Fresh per test
def database():
    db = create_database()
    yield db
    db.cleanup()  # Always runs after test
```

## Pattern: Assertion Message Missing

**Symptom:** Assertion fails with no helpful context

**Fix:** Add message to assertion
```python
# Bad
assert result == expected

# Good
assert result == expected, f"Expected {expected}, got {result}"
```

## Pattern: Test Order Dependency

**Symptom:** Test passes alone, fails in suite (or vice versa)

**Fix:** Find and eliminate shared state
```bash
pytest --randomly-seed=12345  # Detect order dependencies
```
