---
name: review-pr
description: Review a GitHub pull request using the everything-coding-agent multi-perspective PR review workflow. Use when the user invokes review-pr, asks to review a PR, provides a GitHub PR or Issue URL, or wants repository-guided code review.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Review PR

Run the everything-coding-agent PR review workflow for a GitHub PR or linked
Issue. This skill is intentionally exposed as `review-pr` so Codex slash/skill
completion can surface it directly.

For the detailed workflow, use the same steps as
`plugins/everything-coding-agent/skills/review-pr/SKILL.md`.
