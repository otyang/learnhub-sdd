# ADR-001: No User Accounts in MVP

## Status
Approved

## Context
LearnHub aims to demonstrate Spec-Driven Development (SDD) with minimal unnecessary product friction. Traditional authentication (users, passwords, JWTs, OAuth) introduces substantial surface area unrelated to core tutorial reading and progress tracking.

## Decision
For the MVP, LearnHub will **not** include user registration, authentication, login, or passwords. Instead, clients will generate a client-side UUID stored in browser local storage and transmit it via the HTTP header `X-Learner-ID`.

## Alternatives Considered
1. **Full Authentication System (JWT/Sessions):** Rejected due to added complexity for a tutorial demonstration.
2. **IP Address Identification:** Rejected due to NAT/shared IP conflicts across multiple users.

## Consequences
- **Positive:** Zero friction for learners; simpler backend implementation; focused domain logic.
- **Negative:** Clearing browser storage resets progress namespace; progress is tied to a single device/browser.
- **Security Note:** `X-Learner-ID` is not authentication or proof of identity. It acts purely as a data partitioning namespace.
