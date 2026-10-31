# Feature Specification: Lesson Progress

- **Specification ID:** SPEC-PRG-01
- **Title:** Anonymous Lesson Progress Tracking
- **Status:** Approved
- **Version:** 1.0.0

---

## 1. Context & Goals
Learners want to track which lessons they have completed and monitor their percentage completion per tutorial.

---

## 2. Functional Requirements

- **PRG-001:** System MUST require valid UUID in `X-Learner-ID` header for all progress endpoints.
- **PRG-002:** System MUST allow toggling lesson completion (`completed: true | false`).
- **PRG-003:** System MUST dynamically calculate tutorial progress percentage using formula:
  `percentage = floor(completed_published_lessons / total_published_lessons * 100)`
- **PRG-004:** System MUST return 0% progress if total published lessons is 0.

---

## 3. Business Rules
- Completion update requests MUST be idempotent. Marking an already completed lesson as completed MUST NOT duplicate records or return errors.
- Setting `completed: false` removes the completion record for that learner and lesson.
- Completions for unpublished lessons MUST NOT contribute to displayed progress percentage.

---

## 4. API Contract

### Endpoint 1: Get Learner Progress
- **Method:** `GET`
- **Path:** `/api/v1/tutorials/{slug}/progress`
- **Header:** `X-Learner-ID: <UUID>`
- **Response `200 OK`:**
  ```json
  {
    "data": {
      "tutorialSlug": "introduction-to-go",
      "completedPublishedLessons": 1,
      "totalPublishedLessons": 3,
      "percentage": 33
    }
  }
  ```

### Endpoint 2: Toggle Lesson Progress
- **Method:** `PUT`
- **Path:** `/api/v1/tutorials/{slug}/lessons/{lessonId}/progress`
- **Header:** `X-Learner-ID: <UUID>`
- **Request Body:**
  ```json
  {
    "completed": true
  }
  ```
- **Response `200 OK`:**
  ```json
  {
    "data": {
      "lessonId": "c1a93e82-2a54-4a57-897b-9c3167198101",
      "completed": true,
      "updatedAt": "2026-02-01T12:00:00Z"
    }
  }
  ```

---

## 5. Acceptance Criteria

- **AC-PRG-01:** Request missing or with invalid `X-Learner-ID` header returns 400 Bad Request.
- **AC-PRG-02:** Marking lesson complete sets record; subsequent GET progress returns updated count and percentage.
- **AC-PRG-03:** Marking lesson incomplete removes record cleanly (idempotent).
- **AC-PRG-04:** Dynamic calculation matches exact specified integer floor formula.
