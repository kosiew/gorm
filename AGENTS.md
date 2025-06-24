# Repository Agent Guide

This repository is the GORM (Go Object-Relational Mapping) library - a developer-friendly ORM for Go that aims to provide a powerful, flexible, and performant way to interact with databases.

## Project Overview

GORM is a full-featured ORM library that supports:
- Associations (Has One, Has Many, Belongs To, Many To Many, Polymorphism)
- Hooks/Callbacks (Before/After Create/Save/Update/Delete/Find)
- Eager loading with Preload and Joins
- Transactions, Context support, Prepared Statements
- SQL Builder, Auto Migrations, Logger
- Extensible plugin architecture

## Package Structure
- `gorm.io/gorm` - Main package containing core ORM functionality
- `gorm.io/gorm/schema` - Schema parsing and reflection utilities
- `gorm.io/gorm/clause` - SQL clause builders and expressions
- `gorm.io/gorm/callbacks` - Callback system for hooks and operations
- `gorm.io/gorm/logger` - Logging interfaces and implementations
- `gorm.io/gorm/migrator` - Database migration utilities

## Coding Guidelines

### Go Standards
- Follow standard Go conventions (gofmt, golint, go vet)
- Use meaningful variable and function names
- Keep functions focused and single-purpose
- Use proper error handling with descriptive error messages

### GORM-Specific Patterns

#### DB Instance Usage
- Always pass `*DB` instances through method chains
- Use `db.AddError()` for error accumulation
- Implement method chaining pattern where appropriate

#### Callback System
- Register callbacks using the processor pattern
- Use meaningful callback names that describe their purpose
- Implement proper before/after ordering for callbacks
- Handle callback errors gracefully

#### Schema Handling
- Parse models using reflection properly
- Handle different data types and associations
- Validate schema compatibility
- Use proper field tags for database mapping

#### SQL Generation
- Use the clause system for building SQL
- Implement proper escaping and parameter binding
- Support different database dialects
- Generate efficient queries

### Error Handling
- Use `gorm.ErrRecordNotFound` for not found errors
- Accumulate errors using `db.AddError()`
- Provide descriptive error messages with context
- Handle database-specific errors appropriately

### Testing
- Write comprehensive tests for all features
- Use the test suite patterns in `/tests` directory
- Test with different database backends
- Include edge cases and error conditions
- Maintain high test coverage

## Architecture Patterns

### Core Components
1. **DB** - Main database session with configuration
2. **Statement** - SQL statement builder and context
3. **Schema** - Model reflection and metadata
4. **Callbacks** - Hook system for operations
5. **Clauses** - SQL building blocks

### Design Principles
- **Chainable API** - Methods return `*DB` for method chaining
- **Immutable Sessions** - Operations create new sessions
- **Plugin Architecture** - Extensible through interfaces
- **Context Support** - Proper context.Context usage
- **Thread Safety** - Safe for concurrent use

## Common Development Tasks

### Adding New Features
1. Design the API following existing patterns
2. Implement core functionality with proper error handling
3. Add comprehensive tests
4. Document the feature with examples
5. Consider backward compatibility

### Callback Implementation
- Use the processor pattern in `/callbacks`
- Register callbacks with meaningful names
- Implement proper ordering with before/after
- Handle conditional execution with match functions

### SQL Clause Development
- Extend the clause system in `/clause`
- Support multiple database dialects
- Implement proper SQL generation
- Add comprehensive tests for edge cases

### Bug Fixes
1. Write a failing test that reproduces the issue
2. Implement the minimal fix
3. Ensure all tests pass
4. Consider edge cases and backward compatibility

## Formatting
- Format any changed Go files using `gofmt -w` before committing. Tabs are used for indentation.
- If you modify `go.mod` or `go.sum`, run `go mod tidy`.

## Lint and Tests
- Run `golangci-lint run` from the repository root.
- Run the test suite with `GORM_DIALECT=sqlite go test ./...`. This runs both unit tests and the SQLite variant of integration tests.

If commands fail because dependencies cannot be downloaded or databases cannot be started, mention it in the Pull Request description.

## Performance Considerations
- Use prepared statements when beneficial
- Implement proper connection pooling
- Optimize queries for large datasets
- Cache parsed schemas when possible
- Minimize allocations in hot paths

## Security
- Always use parameter binding for user input
- Validate schema definitions
- Implement proper access controls in plugins
- Sanitize table and column names

Remember: GORM aims to be developer-friendly while maintaining performance and reliability. Always consider the developer experience when implementing new features.

