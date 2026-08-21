# Generate, Run & Fix Tests

AI assistants excel at generating test cases — especially edge cases that are easy to miss manually. The key is giving them the implementation to test against, not just a description.

## Checklist

- [ ] I have used AI to generate tests for an existing function
- [ ] I have asked AI to fix a failing test
- [ ] I understand what the AI-generated tests actually test

## Generating Tests

Paste the function and ask for tests covering normal cases, edge cases, and error cases:

```
Write pytest tests for this function.
Cover: normal input, empty list, negative numbers, and non-numeric input.
Use pytest.raises for exception cases.

[paste function here]
```

!!! tip
    Ask specifically for edge cases — the AI defaults to happy-path tests if you don't ask for more.

## Running and Fixing

When a test fails, paste the failure output back into the chat:

```
FAILED tests/test_parser.py::test_empty_input - AssertionError: assert None == []

Fix the test or the implementation — explain which one is wrong and why.
```

Always ask it to explain *which* is wrong — sometimes the test is wrong, not the code.

## Coverage Targets

```bash
pytest tests/ --cov=src --cov-report=term-missing
```

Then ask the AI:

```
These lines are uncovered: src/parser.py lines 45–52.
Generate tests that exercise them.
```
