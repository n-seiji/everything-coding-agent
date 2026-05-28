---
name: draft-pr
description: Create or prepare a draft pull request for the current branch. Use when the user invokes draft-pr or asks to open a draft PR.
allowed-tools: Bash, Read, bash, read_file
---

# Draft PR

Open a draft PR for the current branch.

## Workflow

1. Inspect branch, status, recent commits, and diff.
2. Ensure the branch is pushed. If not, push with upstream after confirming the branch is appropriate.
3. Build a concise PR title and body from the diff, commits, and repository PR template if present.
4. Run `gh pr create --draft --fill` or `gh pr create --draft --title ... --body ...`.
5. Return the PR URL and summarize any checks that were or were not run.

Do not create a PR from `main`, `master`, or `develop` unless explicitly requested.
