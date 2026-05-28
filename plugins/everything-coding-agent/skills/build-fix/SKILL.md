---
name: build-fix
description: Incrementally diagnose and fix project build errors. Use when the user invokes build-fix, asks to fix build failures, or provides TypeScript, JavaScript, or general build errors.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Build Fix

Fix build failures one small change at a time.

## Workflow

1. Identify the project build command from package scripts, README, CI, or repository instructions. Prefer existing commands over guessing.
2. Run the build command and capture the full error output.
3. Group errors by file and root cause. Start with the earliest compile-blocking error.
4. For each fix:
   - Read the surrounding code.
   - Explain the cause briefly.
   - Apply the smallest safe change.
   - Re-run the build or the narrowest relevant check.
5. Stop and report if the same error survives three attempts, a fix creates unrelated failures, or the user asks to pause.
6. Finish with a summary of fixed errors, remaining errors, and verification commands run.

Do not hide failing verification. Do not rewrite unrelated code.
