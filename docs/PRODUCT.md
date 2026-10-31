# LearnHub Product Specification & Vision

## 1. Problem Statement
Many developers and technical learners want lightweight, distraction-free tutorial platforms to learn software concepts. Traditional platforms require mandatory account creation, email validation, and complex onboarding before a learner can even read a lesson or track their reading progress.

## 2. Product Vision
LearnHub is a zero-friction, documentation-first tutorial platform. It provides instant access to structured tutorials and lessons while allowing learners to track their reading progress anonymously via client-side identifiers.

## 3. User Personas & Actors
- **Anonymous Learner:** A visitor browsing tutorials from a web browser. Does not log in or register. Progress is tracked locally via a generated client UUID.
- **Content Publisher (Future Scope / Out of Scope for MVP):** An admin managing tutorials and lessons.

## 4. In-Scope & Out-of-Scope Features

### In-Scope (MVP)
- Browsing published tutorials list and viewing details.
- Reading published lessons in sequential order with previous/next navigation.
- Toggling lesson completion state (complete/incomplete).
- Dynamic progress calculation (count and percentage) per anonymous learner ID.

### Out-of-Scope (Explicitly Excluded)
- User accounts, registration, passwords, email verification, OAuth.
- Monetization, payments, subscriptions, wallets, fintech integrations.
- User profile editing, avatar uploads, comments, social sharing.
- Content authoring/CMS admin UI (content managed via DB seed/migrations in MVP).

## 5. Functional Requirements Overview
- **FR-CATALOGUE:** Learners can list published tutorials and view ordered lesson summaries.
- **FR-LESSONS:** Learners can read published lesson contents and navigate sequentially.
- **FR-PROGRESS:** Learners can set completion status per lesson and retrieve dynamic completion percentages.

## 6. Non-Functional Requirements
- **Performance:** Sub-100ms API response time for tutorial and lesson retrievals.
- **Simplicity:** Single Go backend executable (`cmd/all`).
- **Data Integrity:** Strict database-level unique constraints for slugs, lesson positions, and completion keys.

## 7. Business Rules Summary
1. Unpublished tutorials or lessons must never be exposed over public endpoints.
2. Progress percentage formula: `percentage = floor(completed_published_lessons / total_published_lessons * 100)`.
3. If total published lessons equals 0, progress percentage is 0%.
4. Learner identification relies solely on header `X-Learner-ID`.
