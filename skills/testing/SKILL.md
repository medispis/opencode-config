---
name: testing
description: Designs testing strategies, writes test plans, and generates comprehensive test suites. Use when planning test coverage, designing test architecture, or creating test strategies for new features.
---

# Testing

Designs testing strategies, creates test plans, and generates comprehensive test suites for any codebase.

## How it works

1. Detect the test framework from existing tests and config (Jest, Vitest, pytest, Go test, etc.)
2. Analyze the code to identify testable units, integration points, and edge cases
3. Design a test strategy covering unit, integration, and e2e tests
4. Generate test files following project conventions
5. Run tests and verify they pass

## Test strategy

### Unit tests
- Test individual functions/methods in isolation
- Mock external dependencies
- Cover all branches: happy path, error path, edge cases
- One concept per test

### Integration tests
- Test interactions between components
- Use real dependencies where practical
- Verify data flow and state changes
- Test API endpoints, database interactions

### E2E tests
- Test complete user workflows
- Verify critical user journeys
- Use real browser/API interactions

## Test coverage targets

| Code type | Target |
|-----------|--------|
| Critical business logic | 95%+ |
| API endpoints | 90%+ |
| Utility functions | 80%+ |
| UI components | 70%+ |
| Configuration | N/A (verify, don't test) |

## Test patterns

### Arrange-Act-Assert
```
1. Arrange: Set up test data and mocks
2. Act: Call the function under test
3. Assert: Verify the expected outcome
```

### Test data management
- Use factories for generating test data
- Clean up state between tests
- Don't share data between tests
- Keep test data close to the test

### Mocking strategy
- Mock external services (APIs, databases, file systems)
- Don't mock the code under test
- Use real implementations for utilities and helpers
- Mock time for time-dependent tests

## Rules

- Detect the test framework — don't assume
- Follow the project's existing test file naming conventions
- Place test files where the project expects them
- Use descriptive test names that explain expected behavior
- Test one concept per test function
- If tests exist, extend them rather than rewriting
- Run tests after writing — verify they pass
- If tests fail, report failures clearly with error messages

## Output format

```markdown
## Test Suite

**Framework:** Jest/Vitest/pytest/etc.
**Test files:** [list]
**Total tests:** N
**Coverage:** X%

### Test cases
- [describe]: [what it tests]
- [describe]: [what it tests]

### Results
- Passed: N
- Failed: N
- Skipped: N
```
