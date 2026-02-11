# Backend Architecture

This document defines the guiding principles for the Finance Hub backend so the team can extend features without drifting from the agreed N-tier structure.

## N-Tier Overview
- **Controller layer**: HTTP adapters living under `app/controllers` (NestJS controllers). Responsibilities: validate/parse transport-level data, orchestrate services, map DTOs to domain inputs, and convert service outputs back into DTOs.
- **Service layer**: Application use-case logic. Services coordinate domain models, enforce invariants, and shape transaction boundaries. No framework-specific concerns should leak in.
- **Repository layer**: Data persistence logic (Prisma repositories). Services depend on repository interfaces, enabling mocking and future data-source swaps.

```
Client -> Controller -> Service -> Repository -> Database
```

## DTO Policy
- Controllers **never** expose domain entities. Every request payload and response body must pass through dedicated DTOs.
- DTOs belong to transport subfolders (`dto/requests`, `dto/responses`). Keep them serialization-friendly and immutable when possible.
- Services return domain objects; controllers map them to DTOs before responding.

## SOLID Expectations
1. **Single Responsibility**: Each controller action handles one endpoint; services encapsulate one use case; repositories wrap a single aggregate or table set.
2. **Open/Closed**: Add new behaviors through new classes or compositions instead of editing stable abstractions.
3. **Liskov Substitution**: Repository interfaces must allow alternative implementations (e.g., in-memory fakes for tests).
4. **Interface Segregation**: Prefer focused repository/service interfaces over wide grab-bags.
5. **Dependency Inversion**: Higher layers depend on abstractions. Use NestJS DI tokens to bind interfaces to concrete adapters.

## Docker-First Local Testing
- Local infra (PostgreSQL, Redis) runs via Docker Compose to ensure parity with CI and prod-like environments.
- Provide `docker-compose.dev.yml` targets for databases, and document required ports/env vars in future updates.
- Tests that need external services should rely on the Docker stack; pure unit tests should stub repositories/services instead.

## Unit Testing Strategy
To ensure testability and loose coupling, the following rules apply to unit tests:

1. **Service Interfaces**:
   - Every Service must implement an interface (e.g., `IUserService`) or be designed to be easily mocked.
   - Controllers should depend on the **abstract interface**, not the concrete class.
   - This allows tests to inject mock implementations (using `useValue` or `createMock`) without spinning up actual business logic or database connections.

2. **Scope of Testing**:
   - **Controllers**: Verify that the controller correctly calls the service method with the right arguments and maps the result to the correct DTO. Do *not* test business logic here.
   - **Services**: Test calculations, flow control, and exception handling. Mock all Repositories.
   - **Repositories**: These are generally covered by integration tests (using the Docker database), as mocking a simplified Prisma client usually offers low value.

3. **Mocking**:
   - Use standard Jest mocking or `@golevelup/ts-jest` for creating strict mocks of dependencies.
   - Avoid "partial" mocks that actually execute side effects.

## Request Lifecycle Example
1. **Controller** receives HTTP request, validates DTO (class-validator) and extracts authenticated context.
2. **Service** executes business rule, interacts with repositories, emits domain events (future enhancement) if needed.
3. **Repository** performs Prisma queries/mutations, returning domain models to the service.
4. **Controller** maps service result to response DTO and returns to the client.

## Implementation Checklist
- [ ] Define base repository interfaces per aggregate.
- [ ] Generate DTOs for each new endpoint before writing controller logic.
- [ ] Enforce DI bindings in `app.module.ts` to keep wiring explicit.
- [ ] Extend test suites with both unit (services/controllers) and integration (docker-backed) coverage.

Keep this file updated whenever architectural decisions evolve so new contributors have a single source of truth.
