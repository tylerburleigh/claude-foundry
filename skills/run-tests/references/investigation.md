# Investigation Patterns

## Systematic Approach

1. **Reproduce reliably** - Run test in isolation first
2. **Minimize scope** - Find smallest failing input
3. **Form hypothesis** - What do you think is wrong?
4. **Test hypothesis** - Add logging/assertions to verify
5. **Fix and verify** - Make minimal change, run tests

## Using Debug Output

**Add verbose pytest output:**
```bash
pytest -vvs tests/test_file.py::test_name
```

**Add print statements (captured by pytest):**
```python
def test_something():
    result = function_under_test()
    print(f"DEBUG: result = {result}")
    assert result == expected
```

**Use pytest's built-in debugging:**
```bash
pytest --pdb tests/test_file.py::test_name  # Drop into debugger on failure
```

## Reading Stack Traces

**From bottom to top:**
1. Bottom: The actual error message
2. Above: The line where it occurred
3. Higher: The call chain that led there

**Key information to extract:**
- Which file and line number
- What function was being called
- What arguments were passed (if visible)
