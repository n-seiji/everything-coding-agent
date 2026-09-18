---
name: github-issue-ops
description: Use when filing a GitHub issue, starting work on one, or opening a PR that relates to one.
---

# GitHub issue ops

Policy for the issue/PR lifecycle. `seiji-workflow-prefs` covers the surrounding
workflow (draft PRs, review triads); this skill is only about issue bookkeeping.

## Filing an issue: ask before you set fields

Find out what the repo actually supports (`gh label list`, the milestones API,
`gh issue create --help`, `gh project list --owner <owner>`), then ask the user
about each field that exists: labels, assignee, milestone, project and its
starting status, parent issue, template. One question at a time. Where a single
choice is obviously right, propose it and take a yes/no instead.

Set everything in the `gh issue create` call, and say which fields you could not
set and why — a missing `read:project` scope (`gh auth refresh -s read:project,project`)
or a repo with no milestones is worth reporting, not skipping in silence.

## Starting work: take the assignee slot

When you begin implementing, assign the account behind the current `gh` login
(`gh api user -q .login`). If someone else is already assigned, ask first.

## Status

Move the issue's status when you start and when it lands. With a Projects v2
board, `gh project item-edit` takes `--field`/`--value` by name; `gh project
field-list` shows the available options. Without a board, an `in progress`
label is the fallback — create it once if the repo lacks one, and remove it
after the issue closes if it survives.

If the repo has neither, say so rather than quietly doing nothing.

## Linking a PR

A PR appears in the issue's Development panel only through a closing keyword in
the PR body — `Closes #N`, `Fixes #N`, `Resolves #N`. A comment mentioning the
issue does not. Verify after opening the PR:

```bash
gh pr view <n> --json closingIssuesReferences
```

An empty array means the keyword did not register.

For partial work that must not close the issue, write `Part of #N` and link it
through the issue's Development panel in the web UI. There is no `gh` subcommand
for this, and no GraphQL mutation for linking a PR without a closing reference —
check the schema before assuming otherwise, and fall back to the UI rather than
guessing a mutation name.

## Common violations

- Choosing labels, assignee, or milestone without asking
- Filing the issue but never assigning yourself when work starts
- Leaving status stale after starting or finishing
- Linking a PR by comment only, so it never reaches the Development panel
- Silently skipping a field the repo does not support, instead of reporting it
