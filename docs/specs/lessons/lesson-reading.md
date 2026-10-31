# Feature Specification: Lesson Reading

- **Specification ID:** SPEC-LES-01
- **Title:** Lesson Reading & Navigation
- **Status:** Approved
- **Version:** 1.0.0

---

## 1. Context & Goals
Learners need to view full lesson content and seamlessly navigate between consecutive lessons in a tutorial.

---

## 2. Functional Requirements

- **LES-001:** System MUST return lesson details and body content by tutorial `slug` and `lessonId`.
- **LES-002:** System MUST provide `previousLessonId` and `nextLessonId` navigation references in response.
- **LES-003:** System MUST return 404 if lesson does not belong to the tutorial specified in URL.
- **LES-004:** System MUST return 404 if lesson or parent tutorial is unpublished.

---

## 3. Business Rules
- Previous/next navigation MUST skip unpublished lessons and only reference published lessons.
- If current lesson is first, `previousLessonId` MUST be `null`.
- If current lesson is last, `nextLessonId` MUST be `null`.

---

## 4. API Contract

- **Method:** `GET`
- **Path:** `/api/v1/tutorials/{slug}/lessons/{lessonId}`
- **Response `200 OK`:**
  ```json
  {
    "data": {
      "id": "c1a93e82-2a54-4a57-897b-9c3167198101",
      "tutorialId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "title": "Installing Go",
      "content": "Full lesson markdown content here...",
      "position": 1,
      "navigation": {
        "previousLessonId": null,
        "nextLessonId": "d2b93e82-3b54-4a57-897b-9c3167198102"
      }
    }
  }
  ```

---

## 5. Acceptance Criteria

- **AC-LES-01:** Valid published lesson request returns content and correct prev/next navigation pointers.
- **AC-LES-02:** Attempting to access a lesson with mismatched tutorial slug returns 404.
- **AC-LES-03:** First lesson has `previousLessonId: null`. Last lesson has `nextLessonId: null`.
