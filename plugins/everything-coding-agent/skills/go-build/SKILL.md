---
name: go-build
description: Fix a Go build or static-analysis failure. Use when explicitly invoking the go-build workflow.
allowed-tools: Bash, Read, Write, Edit, bash, read_file, write_file
---

# Go Build

Fix Go build and static analysis failures with minimal changes.

## Workflow

1. Discover repository Go commands from `AGENTS.md`, README, Makefile, mise, CI, and scripts.
2. Run the narrowest relevant command first, usually `go test ./...`, `go build ./...`, `go vet ./...`, or the project wrapper.
3. Parse errors by package and file. Fix compile blockers before lint or cleanup issues.
4. Apply one small fix at a time and rerun the relevant command.
5. Use `go mod tidy` only when module files are actually implicated.
6. Finish with a summary of commands run, errors fixed, and any remaining failures.

Preserve existing architecture and generated-code boundaries.
