---
name: e2e
description: Design, run, or debug end-to-end tests, especially Playwright tests. Use when the user invokes e2e or asks to verify a browser/user journey.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# E2E

Generate, run, or debug end-to-end tests.

## Workflow

1. Identify the target user journey and existing E2E framework. Prefer existing Playwright, Cypress, or project-specific conventions.
2. Inspect test structure, page objects, fixtures, environment setup, and CI commands.
3. For new coverage, write a focused test for the requested journey with stable selectors and clear assertions.
4. For failures, reproduce the failure, inspect screenshots/traces/logs when available, and fix the smallest root cause.
5. Run the narrow E2E command first, then broader checks when needed.
6. Report generated tests, artifacts, failures, and remaining risks.

Do not invent credentials or bypass application auth. Ask for missing secrets only when needed.
