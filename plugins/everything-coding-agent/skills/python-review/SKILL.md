---
name: python-review
description: Review Python changes for correctness, typing, security, style, packaging, and tests. Use when the user invokes python-review or asks for Python-specific review.
allowed-tools: Bash, Read, bash, read_file
---

# Python Review

Review Python code with Python-specific checks.

## Workflow

1. Inspect changed Python files and related tests, config, dependencies, and CI.
2. Read repository guidance before reviewing.
3. Check for:
   - Unsafe `eval`, `exec`, pickle, YAML loading, shell execution, SQL construction, and path handling.
   - Missing or misleading type hints, mutable defaults, broad exception swallowing, and resource leaks.
   - Async misuse, concurrency hazards, and blocking operations in async paths.
   - Packaging, dependency, import, and environment issues.
   - Missing tests for edge cases and error paths.
4. Run or inspect `ruff`, `mypy`, `pytest`, `bandit`, or project wrappers when practical.
5. Return findings first with severity and file:line references.

Do not edit files during review unless asked.
