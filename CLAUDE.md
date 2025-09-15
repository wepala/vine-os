# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**vine-os** is a distributed microservices architecture built around Domain-Driven Design (DDD), CQRS, and Event Sourcing patterns. The project consists of a core DDD library and multiple service modules organized as git submodules.

## Architecture

```
vine-os/
├── core/pericarp/           # Core DDD/CQRS/Event Sourcing library (Go)
├── services/
│   ├── vine-greenpeas/      # Juice Bar bookkeeping service
│   └── vine-iam/           # Identity and Access Management service
└── planning/               # Project planning documents
```

### Core Components

- **Pericarp Core**: A comprehensive Go library implementing DDD, CQRS, and Event Sourcing with clean architecture
- **Services**: Independent microservices that leverage the Pericarp core library
- **Git Submodules**: Each component (core + services) is managed as a separate git repository

## Development Commands

### Core Library (Pericarp)
The main development commands are run from the `core/pericarp/` directory:

```bash
cd core/pericarp

# Build and test
make build                    # Build the library
make test                     # Run all tests (unit + BDD)
make test-unit               # Run unit tests only
make test-bdd                # Run BDD tests with Cucumber
make test-integration        # Run integration tests
make test-performance        # Run performance tests

# Database testing
make test-bdd-sqlite         # Test with SQLite
make test-bdd-postgres       # Test with PostgreSQL (requires POSTGRES_TEST_DSN)

# Development workflow
make fmt                     # Format code
make lint                    # Run golangci-lint
make dev-test               # Format, lint, and test
make ci                     # Full CI pipeline

# Dependency management
make deps                   # Download and tidy dependencies
make generate-mocks         # Generate mocks using moq

# Tools installation
make install-tools          # Install golangci-lint, moq, gosec
```

### Git Submodules Management

```bash
# Initialize submodules after cloning
git submodule update --init --recursive

# Update all submodules to latest
git submodule update --remote

# Update specific submodule
git submodule update --remote core/pericarp
git submodule update --remote services/vine-greenpeas
git submodule update --remote services/vine-iam
```

## Technology Stack

- **Language**: Go 1.22+ (core library and services)
- **Architecture**: DDD, CQRS, Event Sourcing, Clean Architecture
- **Dependencies**:
  - Watermill (event streaming)
  - GORM (ORM with SQLite/PostgreSQL support)
  - Uber Fx (dependency injection)
  - Viper (configuration)
  - Cucumber/Godog (BDD testing)
- **Testing**: Unit tests, BDD scenarios, integration tests, performance tests

## Key Patterns

### Domain Events
The project uses a `StandardEvent` pattern for flexible event creation:

```go
// Single factory for all event types
event := domain.NewEvent("user-123", "User", "Created", map[string]interface{}{
    "email": "john@example.com",
    "name":  "John Doe",
})
```

### Aggregate Root
Built-in `Entity` struct provides event sourcing capabilities:

```go
type User struct {
    domain.Entity  // Embeds ID, version, sequenceNo, events management
    email    string
    name     string
}
```

### Command/Query Separation
Separate buses for commands and queries with middleware support:

```go
commandBus.Register("UpdateUserEmail", handler,
    application.LoggingMiddleware[UpdateUserEmailCommand, struct{}](),
    application.ValidationMiddleware[UpdateUserEmailCommand, struct{}](),
)
```

## Service Structure

- **vine-greenpeas**: Juice bar bookkeeping application
- **vine-iam**: Identity and Access Management service

Each service is independently developed and maintained in its own repository but leverages the shared Pericarp core library.

## Testing Strategy

- **Unit Tests**: Fast, isolated tests for individual components
- **BDD Tests**: Cucumber scenarios for behavior verification
- **Integration Tests**: End-to-end testing with real databases
- **Performance Tests**: Benchmarking and profiling
- **Database Support**: Tests run against both SQLite (dev) and PostgreSQL (prod)

## Development Workflow

1. Work within individual submodules (`core/pericarp/`, `services/*/`)
2. Use `make dev-test` for rapid development cycles
3. Run full test suites before committing
4. Update parent repository to track submodule changes
5. Follow clean architecture principles and DDD patterns