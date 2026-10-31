# LearnHub Engineering Standards

## 1. Go Idioms & Code Conventions
- Follow standard Go project layout practices.
- Use explicit error handling (`if err != nil`). Never ignore errors.
- Keep functions small and single-purpose.
- Avoid global mutable state. Inject dependencies explicitly via constructors (e.g., `NewService(repo Repository)`).

## 2. HTTP & API Conventions
- Standard JSON envelope for successful responses:
  ```json
  {
    "data": { ... }
  }
  ```
- Standard JSON envelope for error responses:
  ```json
  {
    "error": {
      "code": "ERROR_CODE_NAME",
      "message": "Human-readable description."
    }
  }
  ```
- HTTP status codes:
  - `200 OK`: Successful retrieval or update.
  - `400 Bad Request`: Validation failure or missing header/field.
  - `404 Not Found`: Entity missing or unpublished.
  - `500 Internal Server Error`: Unhandled server exception.

## 3. Database & SQL Conventions
- Use explicit column names in SQL SELECT statements (no `SELECT *`).
- Foreign key relations must be defined explicitly in migration scripts.
- Use PostgreSQL parameterized queries (`$1, $2`) to prevent SQL injection.

## 4. Testing Standards
- Table-driven tests for domain logic and calculations.
- Integration tests for API endpoints using `net/http/httptest`.
- All acceptance criteria in feature specs must have direct corresponding unit or integration test coverage.
