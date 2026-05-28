---
name: verify
description: Run project verification checks and report readiness. Use when the user invokes verify or asks whether the current work is ready.
allowed-tools: Bash, Read, bash, read_file
---

# Verify

Run appropriate checks for the current repository state.

## Workflow

1. Discover verification commands from repository instructions, README, package scripts, Makefile, mise, CI, and language config.
2. Choose mode from the user request:
   - `quick`: build and type checks.
   - `pre-commit`: focused lint/test checks for changed files.
   - `pre-pr` or `full`: build, lint, tests, and security checks when available.
3. Run checks in dependency order: build/type, lint, tests, coverage, security/secrets, git status.
4. Stop early only when a failure blocks later checks.
5. Report:

   ```text
   VERIFICATION: PASS/FAIL
   Build:
   Types:
   Lint:
   Tests:
   Security:
   Git:
   Ready for PR:
   ```

Include exact commands and any failures.
