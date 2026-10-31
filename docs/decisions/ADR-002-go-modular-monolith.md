# ADR-002: Go Modular Monolith Architecture

## Status
Approved

## Context
We need an application architecture that is easy to deploy, simple to test, and maintains clean domain boundaries without microservice overhead.

## Decision
Build LearnHub as a **Go Modular Monolith** with a single composition executable (`cmd/all`). Domain boundaries (`tutorials`, `lessons`, `progress`) are enforced at package boundaries inside `internal/`.

## Alternatives Considered
1. **Microservices Architecture:** Rejected due to deployment, network, and operational complexity.
2. **Single Flat Package:** Rejected due to risk of tight coupling between domains.

## Consequences
- **Positive:** Single binary deployment; simple testing setup; strong language-level boundary enforcement.
- **Negative:** Requires disciplined code reviews to prevent cross-module database access.
