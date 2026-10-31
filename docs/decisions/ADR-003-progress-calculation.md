# ADR-003: Derive Progress Percentage Instead of Storing It

## Status
Approved

## Context
Learner progress percentage depends on both completed lessons and the total number of published lessons. Storing progress percentage as a column in the database creates data synchronization risks when new lessons are added or published.

## Decision
Progress percentage will be **dynamically calculated on request** using completion records and published lesson counts:
`percentage = floor(completed_published_lessons / total_published_lessons * 100)`
The database stores only completion state (`lesson_completions` table).

## Alternatives Considered
1. **Stored Aggregated State:** Store `progress_percent` column updated via DB triggers or app logic. Rejected due to cache invalidation risks when lesson publication state changes.

## Consequences
- **Positive:** Single source of truth; zero risk of stale percentage values.
- **Negative:** Requires lightweight dynamic SQL aggregation on progress queries.
