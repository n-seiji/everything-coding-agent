---
name: cp
description: Commit and push the current changes with a clear conventional commit message. Use when the user invokes cp or asks to commit and push local work.
allowed-tools: Bash, Read, bash, read_file
---

# Commit And Push

Create one intentional commit and push it.

## Workflow

1. Inspect `git status --short`, `git diff --stat`, and relevant diffs.
2. If on `main`, `master`, or `develop`, create a feature branch unless the user explicitly asked to commit directly there.
3. Stage only relevant files. Do not include unrelated user changes.
4. Commit with a conventional commit message matching the change.
5. Push the current branch to origin.
6. Report the commit hash, branch, and push result.

If there are no changes, stop and report that nothing was committed.
