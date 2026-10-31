# Lessons Domain Boundary

## Responsibilities
- Managing lesson titles, content, sequence position, and publication status.
- Enforcing tutorial-lesson ownership (a lesson belongs to exactly one tutorial).
- Calculating sequential navigation pointers (`previousLessonId`, `nextLessonId`).

## Key Invariants
- A lesson is readable only if both the parent tutorial and the lesson are `published`.
- Lessons within a tutorial have a strict, deterministic sequence order based on `position`.
- Sequential navigation skips unpublished lessons.
