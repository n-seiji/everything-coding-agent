---
name: go-test
description: Add or run Go tests using idiomatic table-driven testing and focused verification. Use when the user invokes go-test or asks for Go test coverage.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Go Test

Use focused Go testing to verify behavior.

## Workflow

1. Identify the package, behavior, and existing test style.
2. For new behavior or bug fixes, write a failing test first when feasible.
3. Prefer table-driven tests with clear case names, explicit expected values, and meaningful error messages.
4. Use mocks, fixtures, and integration helpers already present in the repository.
5. Run the narrow package test first, then broader project tests if the blast radius warrants it.
6. Report test commands, pass/fail status, and coverage gaps.

Avoid brittle sleeps, network calls, and order-dependent assertions unless the project already uses them intentionally.
