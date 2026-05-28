---
name: update-docs
description: Update project documentation from current source-of-truth files and repository behavior. Use when the user invokes update-docs or asks to refresh docs.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Update Docs

Bring documentation in line with the current repository.

## Workflow

1. Identify the requested documentation scope.
2. Read source-of-truth files such as README, package scripts, Makefile, mise, Docker Compose, `.env.example`, OpenAPI specs, CI, and existing docs.
3. Update only docs that are stale or missing relevant information.
4. Keep commands, paths, repository names, and installation steps exact.
5. Remove obsolete references when they are clearly replaced.
6. Verify links and command names where practical.
7. Summarize changed docs and any unverified assumptions.
