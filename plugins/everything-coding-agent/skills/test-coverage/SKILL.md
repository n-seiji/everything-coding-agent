---
name: test-coverage
description: Add tests for meaningful coverage gaps. Use when explicitly invoking the test-coverage workflow.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Test Coverage

Find meaningful coverage gaps and add tests.

## Workflow

1. Identify the project's coverage command from package scripts, Makefile, CI, README, or repository instructions.
2. Run the narrow or full coverage command as appropriate.
3. Inspect coverage reports and changed files, prioritizing business logic, edge cases, error handling, and security-sensitive paths.
4. Add focused tests that assert behavior, not implementation details.
5. Re-run the relevant coverage command.
6. Report before/after coverage where available, tests added, and remaining gaps.

Do not chase arbitrary percentages with low-value tests.
