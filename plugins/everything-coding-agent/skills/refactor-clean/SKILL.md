---
name: refactor-clean
description: Safely refactor or remove dead code with verification. Use when the user invokes refactor-clean or asks to clean up unused code.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Refactor Clean

Clean up code while preserving behavior.

## Workflow

1. Identify the cleanup target and current behavior.
2. Use repository tools for dead code or dependency analysis when available, such as `knip`, `depcheck`, `ts-prune`, compiler errors, or language-specific linters.
3. Classify removals as safe, caution, or dangerous based on entry points, exports, runtime wiring, and public API usage.
4. Apply small refactors with tests or build checks between meaningful steps.
5. Never delete config, generated files, migrations, public API, or runtime entry points solely because a generic tool marks them unused.
6. Finish with removed items, verification run, and residual risk.
