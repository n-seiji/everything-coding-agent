---
name: tdd
description: Implement changes with a red-green-refactor test-driven workflow. Use when the user invokes tdd or asks to write tests first.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# TDD

Drive implementation through tests.

## Workflow

1. Clarify the behavior and observable contract.
2. Inspect existing test patterns and choose the narrowest test layer that proves the behavior.
3. Write or modify a test that fails for the intended reason.
4. Run the narrow test and confirm the failure.
5. Implement the smallest code change to pass.
6. Re-run the test, then relevant broader checks.
7. Refactor only with tests green.

If a true red test is impractical, explain why and choose the closest verification path.
