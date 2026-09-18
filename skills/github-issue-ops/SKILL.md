---
name: github-issue-ops
description: Use when creating, starting work on, updating the status of, or linking a PR to a GitHub issue via `gh`. Covers hearing out issue metadata before creation, setting yourself as assignee when you start work, keeping status (Projects field or label) in sync, and making sure a PR shows up in the issue's Development panel instead of only a comment.
---

# GitHub issue ops

Operational steps for the GitHub issue/PR lifecycle with the `gh` CLI. This skill is about *mechanics* (which flags, which API calls, how to verify); `seiji-workflow-prefs` owns the *policy* (draft PRs, `Closes #N` keyword choice, review triads) — read that skill's "Iron rules" alongside this one, especially the PR-linking rule it already states.

## When to use this skill

Any time you're about to run `gh issue create`, `gh issue edit`, `gh pr create`, or touch a GitHub Project for issue/PR bookkeeping.

## 1. Creating an issue: hear out every settable field first

Never guess labels/assignee/milestone/project/parent. Before `gh issue create`, discover what the repo actually supports, then ask the user field-by-field.

```bash
# Labels available in this repo
gh label list

# Milestones available in this repo
gh api repos/{owner}/{repo}/milestones

# Templates available
gh issue create --help    # shows --template, --type, --parent, --project, --blocked-by/--blocking

# Projects (v2) the org/user owns — needs `read:project` scope
gh project list --owner <owner>
```

If `gh project list` fails with:

```
error: your authentication token is missing required scopes [read:project]
To request it, run:  gh auth refresh -s read:project
```

tell the user Projects can't be queried until they run `gh auth refresh -s read:project,project` (the `project` scope is also needed to *add* items later). Don't silently skip Projects — report that it's unavailable and why.

Then ask the user, one item at a time, for each field that actually exists in the repo:

- Label(s) — show the list from `gh label list`, ask which apply. If there's only one obviously-fitting label, name it and ask for a yes/no instead of an open question.
- Assignee — usually the user themselves; confirm rather than assume `@me`.
- Milestone — only ask if `gh api repos/{owner}/{repo}/milestones` returns a non-empty list.
- Project (and its status field's starting value) — only ask if `gh project list --owner <owner>` succeeded.
- Parent issue (sub-issue) — ask if this issue is part of a larger tracked issue.
- Template — if `.github/ISSUE_TEMPLATE/` has templates, ask which one (or none).

Create with everything confirmed in one call:

```bash
gh issue create \
  --title "..." \
  --body "..." \
  --label "bug" --label "help wanted" \
  --assignee "n-seiji" \
  --milestone "v1.2" \
  --project "Roadmap" \
  --parent 100
```

Report explicitly which fields you set and which you *couldn't* (no milestone in repo, no Projects scope, etc.) — don't just leave them out quietly.

## 2. Starting work: assign yourself

When you actually start implementing an issue (not just filing it), set the assignee to the current `gh` account:

```bash
gh api user -q .login
gh issue edit <n> --add-assignee "<login>"   # or --add-assignee "@me"
```

If the issue already has a different assignee, don't overwrite silently — confirm with the user first (they may be tracking someone else's issue).

## 3. Status updates: Projects field, or a label fallback

### With a Projects v2 board

The by-name form is the primary path — no ID lookups needed:

```bash
# Get the project number
gh project list --owner <owner>

# Move the issue/PR's "Status" field to "In Progress"
gh project item-edit <project-number> --owner <owner> \
  --url <issue-or-pr-url> --field "Status" --value "In Progress"
```

If the field or option names aren't obvious, list them first:

```bash
gh project field-list <project-number> --owner <owner>
```

For scripted/machine use only (not needed for normal by-hand updates), the node-ID form takes `--id` (item), `--field-id`, `--project-id`, and `--single-select-option-id` instead of `--url`/`--field`/`--value`; get the IDs with `--format json` on `field-list`, `item-list`, and `gh project view <n> --owner <owner> --format json --jq .id`.

Move it to "Done" (or whatever the board's terminal state is called) the same way once the PR merges, if it isn't automated by board workflow rules.

### Without Projects (label fallback)

Use an `in progress` label — e.g. villa's repo already has one (`in progress`, `#fbca04`). If a repo you're in doesn't, create it once:

```bash
gh label create "in progress" --color fbca04 --description "in progress"
gh issue edit <n> --add-label "in progress"
```

After the PR merges and the issue auto-closes, check whether the label survived the close and remove it if so:

```bash
gh issue edit <n> --remove-label "in progress"
```

### No Projects, no desire for a label

State this plainly instead of doing nothing silently: "This repo has no Projects board and no status-label convention, so I'm skipping the status update."

## 4. Linking a PR into the issue's Development panel

A PR only shows in an issue's Development panel (and only auto-closes the issue) if the body contains a closing keyword — `Closes #N` / `Fixes #N` / `Resolves #N` — or the PR is linked via the GraphQL/UI mechanism below. A plain-text mention or a comment saying "related to #N" does **not** appear in Development.

### The PR should close the issue

Put one line in the PR body (see `seiji-workflow-prefs` for which keyword to choose):

```
Closes #42
```

### The PR should NOT close the issue (partial work, split across PRs)

Write `Part of #N` in the body (this avoids auto-close) and then link it into Development by hand, since `gh` has no dedicated subcommand for this. Two options, in order of preference:

1. **Manual, via the web UI** — open the issue, its Development panel → "Link a pull request" → pick the PR. This is guaranteed correct; report to the user that you did it this way, since there's no scriptable `gh` command for it as of this writing.
2. **Investigate the GraphQL surface before scripting it.** Do not write a `linkProjectV2...`-style mutation from memory — that name doesn't exist for this purpose. Check what's actually mutable first:

   ```bash
   # List every mutation whose name mentions "issue" or "link"
   gh api graphql -f query='{ __schema { mutationType { fields(includeDeprecated: false) { name } } } }' \
     | tr ',' '\n' | grep -i -E 'issue|link'
   ```

   As of this writing that list includes `addCloseIssueReferences` / `removeCloseIssueReferences` (for the closing-keyword relationship, not a plain link), `addSubIssue` / `removeSubIssue` (parent/child), and `createLinkedBranch` (branch, not PR) — nothing named for "link a PR to an issue without closing it." If a future schema check turns up something suitable, use it; otherwise fall back to the manual UI step rather than guessing a mutation name.

### Verify after creating the PR

```bash
gh pr view <n> --json closingIssuesReferences
```

Example, run against villa PR #63 which already carries `Closes #64`:

```json
{"closingIssuesReferences":[{"number":64, "...": "..."}]}
```

An empty array means the keyword didn't register — check the issue number and that it's in the same repo before reporting the PR as linked.

## Common violations (red flags — stop on sight)

- Picking labels/assignee/milestone/project without asking the user first
- Assigning issue metadata but not yourself when you actually start the work
- Leaving an issue's status stale (no Projects update, no label) after starting or finishing
- Linking a PR to an issue only through a comment, so it never appears in the Development panel
- A repo with no Projects and no status-label convention — silently doing nothing instead of saying so
- Inventing a GraphQL mutation name instead of checking the schema or falling back to the UI
