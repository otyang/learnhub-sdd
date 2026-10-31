# Feature Specification: Tutorial Catalogue

- **Specification ID:** SPEC-CAT-01
- **Title:** Tutorial Catalogue & Details
- **Status:** Approved
- **Version:** 1.0.0

---

## 1. Context & Goals
Learners need to browse available published tutorials and view details of a specific tutorial including its ordered lesson outline.

### Goals
- Expose list of published tutorials.
- Expose tutorial details by unique slug.
- Include ordered lesson metadata summaries in tutorial details.

### Non-Goals
- Exposing draft or unpublished tutorials.
- Admin content editing endpoints.

---

## 2. Functional Requirements

- **CAT-001:** System MUST return a list of all published tutorials ordered by `published_at` descending.
- **CAT-002:** System MUST return tutorial details and ordered published lesson summaries when requested by valid `slug`.
- **CAT-003:** System MUST return 404 Not Found if requested slug does not exist or tutorial status is not `published`.

---

## 3. Business Rules
- Tutorials with status `draft` or `archived` MUST NOT be returned in API responses.
- Lesson summaries within tutorial details MUST be ordered by lesson `position` ascending.

---

## 4. API Contract

### Endpoint 1: List Tutorials
- **Method:** `GET`
- **Path:** `/api/v1/tutorials`
- **Response `200 OK`:**
  ```json
  {
    "data": [
      {
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "slug": "introduction-to-go",
        "title": "Introduction to Go",
        "summary": "Learn the basics of Go programming.",
        "publishedAt": "2026-01-15T10:00:00Z",
        "lessonCount": 3
      }
    ]
  }
  ```

### Endpoint 2: Get Tutorial Details
- **Method:** `GET`
- **Path:** `/api/v1/tutorials/{slug}`
- **Response `200 OK`:**
  ```json
  {
    "data": {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "slug": "introduction-to-go",
      "title": "Introduction to Go",
      "summary": "Learn the basics of Go programming.",
      "publishedAt": "2026-01-15T10:00:00Z",
      "lessons": [
        {
          "id": "c1a93e82-2a54-4a57-897b-9c3167198101",
          "title": "Installing Go",
          "position": 1
        }
      ]
    }
  }
  ```

---

## 5. Acceptance Criteria

- **AC-CAT-01:** Requesting `GET /api/v1/tutorials` returns only published tutorials.
- **AC-CAT-02:** Requesting `GET /api/v1/tutorials/{slug}` with valid published slug returns 200 with ordered lessons.
- **AC-CAT-03:** Requesting `GET /api/v1/tutorials/{slug}` with an unpublished or missing slug returns 404 error envelope.
