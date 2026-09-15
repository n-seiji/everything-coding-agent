---
name: plan
description: Create an implementation plan without editing code. Use when explicitly invoking the plan workflow.
allowed-tools: Bash, Read, bash, read_file
---

# Plan

Produce a concrete implementation plan and wait for user confirmation before editing.

## Workflow

1. Restate the goal and constraints.
2. Inspect the repository enough to identify architecture, ownership boundaries, commands, and risk.
3. Break the work into ordered steps with dependencies.
4. Call out risks, validation commands, migrations, compatibility concerns, and open questions.
5. End by asking for confirmation or the specific decision needed.

Do not modify files while using this skill unless the user explicitly says to proceed.
