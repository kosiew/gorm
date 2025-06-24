# Repository Agent Guide

This repository is a Go module. The following guidelines apply when you modify anything in this repo.

## Formatting
- Format any changed Go files using `gofmt -w` before committing. Tabs are used for indentation.
- If you modify `go.mod` or `go.sum`, run `go mod tidy`.

## Lint and Tests
- Run `golangci-lint run` from the repository root.
- Run the test suite with `GORM_DIALECT=sqlite go test ./...`. This runs both unit tests and the SQLite variant of integration tests.

If commands fail because dependencies cannot be downloaded or databases cannot be started, mention it in the Pull Request description.

