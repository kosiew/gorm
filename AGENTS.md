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

### Go Development Best Practices

#### Code Organization
- Organize code into logical packages with clear responsibilities
- Keep package names short, lowercase, and descriptive
- Avoid package names that conflict with standard library
- Use internal packages for code that shouldn't be imported externally
- Group related types and functions together in the same file

#### Naming Conventions
- Use camelCase for unexported names, PascalCase for exported names
- Use descriptive names that explain the purpose, not the type
- Prefer `userID` over `userId` for consistency with Go style
- Use single-letter variables only for short scopes (loops, receivers)
- Name interfaces with `-er` suffix when they describe behavior (e.g., `Reader`, `Writer`)

#### Error Handling
- Always handle errors explicitly; never ignore them
- Return errors as the last return value
- Use `errors.New()` or `fmt.Errorf()` for creating errors
- Wrap errors with additional context using `fmt.Errorf("context: %w", err)`
- Check for specific error types using `errors.Is()` and `errors.As()`
- Don't use panic for normal error conditions

#### Memory Management
- Avoid unnecessary allocations in hot paths
- Reuse slices and maps when possible with proper capacity
- Use sync.Pool for expensive-to-allocate objects
- Be mindful of goroutine leaks - ensure they can exit
- Close resources (files, connections) using defer statements

#### Concurrency Best Practices
- Use channels to communicate between goroutines
- Prefer `sync.Mutex` over channels for protecting shared state
- Always use `context.Context` for cancellation and timeouts
- Avoid sharing mutable state; prefer immutable data structures
- Use `sync.WaitGroup` to wait for goroutines to complete
- Handle goroutine lifecycle properly to prevent leaks

#### Performance Optimization
- Profile before optimizing; use `go test -bench` and `go tool pprof`
- Prefer string builders (`strings.Builder`) over string concatenation
- Use appropriate data structures (map vs slice based on access patterns)
- Minimize interface{} usage in favor of type-safe alternatives
- Cache expensive computations when appropriate
- Use build constraints for platform-specific optimizations

#### Testing Best Practices
- Write table-driven tests for multiple test cases
- Use meaningful test names that describe the scenario
- Test both happy path and error conditions
- Use testify/assert for cleaner test assertions
- Mock external dependencies for unit tests
- Use integration tests for end-to-end validation
- Maintain test coverage above 80% for critical paths

#### Code Quality
- Run `go vet`, `golint`, and `golangci-lint` regularly
- Use `go mod tidy` to keep dependencies clean
- Write self-documenting code with clear variable names
- Add package-level documentation for exported types and functions
- Use `//go:generate` for code generation when appropriate
- Keep functions under 50 lines when possible

#### Dependency Management
- Use Go modules (`go.mod`) for dependency management
- Pin dependency versions for reproducible builds
- Regularly update dependencies and check for security vulnerabilities
- Minimize external dependencies; prefer standard library when possible
- Use `replace` directives carefully and document why they're needed

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

### GORM-Specific Go Best Practices

#### Model Design
- Use proper struct tags for database mapping (`gorm:"column:name"`)
- Implement `Tabler` interface for custom table names
- Use embedded structs for common fields (ID, timestamps)
- Define associations clearly with proper foreign key relationships
- Use pointer types for nullable fields to distinguish zero values from null

#### Database Operations
- Always use transactions for multi-step operations
- Use `db.Session(&gorm.Session{})` to create isolated sessions
- Implement proper connection pooling configuration
- Use prepared statements for repeated queries
- Handle database-specific errors gracefully

#### Query Optimization
- Use `Select()` to limit columns when not all fields are needed
- Implement proper pagination with `Limit()` and `Offset()`
- Use `Preload()` judiciously to avoid N+1 query problems
- Consider using `Joins()` instead of `Preload()` for better performance
- Index frequently queried columns in your database schema

#### Context and Cancellation
- Always pass `context.Context` to database operations
- Use `db.WithContext(ctx)` for request-scoped operations
- Implement proper timeout handling for long-running queries
- Respect context cancellation in custom callbacks
- Use `context.WithTimeout()` for operations with time limits

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

