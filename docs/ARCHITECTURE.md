# LearnHub Architecture Specification

## 1. Architectural Style: Modular Monolith
LearnHub is built as a single Go executable (`cmd/all`) structured as a **modular monolith**. 

```text
               +----------------------------------+
               |          HTTP Clients            |
               +----------------------------------+
                                | (HTTP / JSON API)
                                v
               +----------------------------------+
               |        pkg/httpserver            |
               | (Routing, CORS, Error Handling) |
               +----------------------------------+
                                |
        +-----------------------+-----------------------+
        |                       |                       |
        v                       v                       v
+---------------+       +---------------+       +---------------+
|   tutorials   |       |    lessons    |       |   progress    |
|   Module      |       |    Module     |       |   Module      |
+---------------+       +---------------+       +---------------+
        |                       |                       |
        +-----------------------+-----------------------+
                                |
                                v
               +----------------------------------+
               |           pkg/database           |
               |         (PostgreSQL Pool)        |
               +----------------------------------+
```

## 2. Module Boundaries & Responsibilities

- **`internal/tutorials`**: Owns tutorial metadata, list filtering by publication status, and retrieving tutorial details by slug.
- **`internal/lessons`**: Owns lesson content retrieval, ordering validation, and prev/next navigation logic.
- **`internal/progress`**: Owns anonymous completion state toggling and progress percentage dynamic calculation.
- **`pkg/`**: Shared non-domain infrastructure utilities (logging, HTTP web server helpers, DB connection setup, UUID handling).

## 3. Dependency Direction & Invariants
- High-level domain logic (`internal/*`) depends on shared infrastructure (`pkg/*`).
- Modules in `internal/` MUST NOT directly query or mutate database tables owned by another domain module.
- Composition Root is localized entirely within `cmd/all/main.go`.

## 4. HTTP Request Lifecycle
1. HTTP Request hits `pkg/httpserver`.
2. Middleware extracts and validates standard headers (e.g., `X-Learner-ID`).
3. Request is routed to specific handler in `internal/<module>`.
4. Module handler calls service/repository layer.
5. Response is wrapped in a standard JSON envelope: `{"data": ...}` or `{"error": ...}`.
