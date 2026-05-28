---
name: code-review
description: Review local code changes for correctness, security, maintainability, tests, and repository rule compliance. Use when the user invokes code-review or asks for a review of uncommitted changes.
allowed-tools: Bash, Read, bash, read_file
---

# Code Review

Review the current local diff in a code-review stance.

## Workflow

1. Inspect the diff with `git status --short`, `git diff --stat`, and `git diff`.
2. Discover repository guidance before judging the code: `AGENTS.md`, `CLAUDE.md`, `.cursor/rules/**`, `.github/copilot-instructions.md`, `README.md`, `CONTRIBUTING.md`, `docs/**`, PR templates, and CI/lint/test configuration.
3. Review for:
   - Correctness and behavioral regressions.
   - Security issues such as secrets, injection, path traversal, auth bypass, unsafe command execution, and sensitive logging.
   - Error handling, edge cases, concurrency, data integrity, and observability.
   - Missing or weak tests.
   - Violations of discovered repository rules.
4. Report findings first, ordered by severity with file and line references.
5. Include open questions, then a short summary. If no issues are found, say so and mention residual test risk.

Do not modify files unless the user explicitly asks for fixes.
