# Tool Selection

## By Failure Type

| Failure Type | Primary Tool | Why |
|--------------|--------------|-----|
| Assertion mismatch | Codex | Code-level bug analysis |
| Exceptions | Codex | Precise code review |
| Import/packaging | Gemini | Framework expertise |
| Fixture issues | Gemini | Pytest scoping knowledge |
| Timeout/performance | Gemini + Cursor | Strategy + discovery |
| Flaky tests | Gemini + Cursor | Diagnosis + state deps |
| Multi-file issues | Cursor | Discovery + synthesis |

## Prompt Templates

**For assertion failures:**
```
Test {test_name} in {file} is failing with an assertion error.
Expected: {expected}
Actual: {actual}

The test verifies: {test_purpose}
The implementation code is: {relevant_code}

What is likely causing the mismatch?
```

**For exception failures:**
```
Test {test_name} raises {error_type}: {error_message}

Stack trace:
{stack_trace}

What is likely causing this exception and how should it be fixed?
```
