# LearnHub Implementation Plan

## Phase 1: Repository Foundation & Core Infrastructure
- **Objectives:** Establish Go module, HTTP server boilerplate, config loader, database connection pool, and middleware.
- **Tasks:**
  1. Initialize `go.mod` and directory structure.
  2. Implement `pkg/config`, `pkg/database`, `pkg/httpserver`, and `pkg/id`.
  3. Create global error response envelope handling.
- **Exit Criteria:** Server starts on specified port, connects to PostgreSQL, and serves `/health`.

## Phase 2: Tutorial Catalogue Domain
- **Objectives:** Implement tutorial listing and details by slug.
- **Tasks:**
  1. Implement `internal/tutorials` domain model, repository, and service.
  2. Implement HTTP handlers for `/api/v1/tutorials` and `/api/v1/tutorials/{slug}`.
  3. Filter out draft/unpublished tutorials.
- **Exit Criteria:** Pass AC-CAT-01 through AC-CAT-04 integration tests.

## Phase 3: Lesson Reading Domain
- **Objectives:** Implement lesson content viewing and prev/next navigation.
- **Tasks:**
  1. Implement `internal/lessons` service and repository.
  2. Implement HTTP handler for `/api/v1/tutorials/{slug}/lessons/{lessonId}`.
  3. Validate tutorial-lesson relationship and ordering calculation.
- **Exit Criteria:** Pass AC-LES-01 through AC-LES-04 integration tests.

## Phase 4: Anonymous Progress Tracking
- **Objectives:** Implement completion toggling and dynamic progress aggregation.
- **Tasks:**
  1. Implement `X-Learner-ID` extraction middleware.
  2. Implement `internal/progress` repository and completion toggle handler.
  3. Implement progress summary calculation (`percentage = floor(completed/total * 100)`).
- **Exit Criteria:** Pass AC-PRG-01 through AC-PRG-06 integration tests.

## Phase 5: Testing & Hardening
- **Objectives:** End-to-end integration tests, race detection, and OpenAPI compliance verification.
- **Exit Criteria:** CI pipeline executes cleanly and all specifications are validated.
