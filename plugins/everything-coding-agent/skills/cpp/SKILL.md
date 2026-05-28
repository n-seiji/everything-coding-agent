---
name: cpp
description: Commit, push, and open a draft pull request. Use when the user invokes cpp or asks to commit, push, and create a PR.
allowed-tools: Bash, Read, bash, read_file
---

# Commit Push PR

Commit the current work, push the branch, and open a draft PR.

## Workflow

1. Inspect `git status --short`, `git diff --stat`, and relevant diffs.
2. If on `main`, `master`, or `develop`, create a feature branch unless the user explicitly requested direct commit.
3. Stage only relevant files. Do not include unrelated user changes.
4. Commit with a conventional commit message.
5. Push the branch to origin.
6. Create a draft PR with `gh pr create --draft --fill`, adjusting title/body when useful.
7. Report the commit hash, branch, and PR URL.

If any step fails, stop and report the exact failed step.
