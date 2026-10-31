# Progress Domain Boundary

## Responsibilities
- Recording lesson completion state per anonymous learner (`X-Learner-ID`).
- Aggregating completed published lessons against total published lessons.
- Calculating progress percentages.

## Key Invariants
- Progress is strictly isolated per `learner_id` UUID namespace.
- Completion records are idempotent (setting `completed: true` multiple times creates only one row).
- Completions for unpublished lessons do not count toward calculated progress.
