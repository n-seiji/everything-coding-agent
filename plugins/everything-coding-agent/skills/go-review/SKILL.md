---
name: go-review
description: Review Go changes for idioms, correctness, concurrency, context usage, errors, security, and tests. Use when the user invokes go-review or asks for Go-specific review.
allowed-tools: Bash, Read, bash, read_file
---

# Go Review

Review Go code with Go-specific checks.

## Workflow

1. Inspect changed Go files with `git diff -- '*.go'` plus related generated schemas, SQL, config, and tests.
2. Read repository guidance and Go conventions before reviewing.
3. Check for:
   - Incorrect error handling, missing context, lost cancellation, and panic misuse.
   - Data races, goroutine leaks, channel misuse, and lock ordering issues.
   - SQL/command injection, unsafe path handling, auth boundary mistakes, and sensitive logs.
   - Non-idiomatic APIs, overbroad interfaces, missing table tests, and weak edge coverage.
   - Performance issues such as missing map/slice capacity when sizes are known.
4. Run or inspect `go test`, `go vet`, `golangci-lint`, `staticcheck`, or project wrappers when practical.
5. Return findings first with severity and file:line references.

Do not edit files during review unless asked.
