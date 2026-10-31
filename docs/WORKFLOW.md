# Spec-Driven Development (SDD) Workflow

## 1. SDD Lifecycle Stages

```text
[ 1. Propose ] ──> [ 2. Specify ] ──> [ 3. Review ] ──> [ 4. Approve ] ──> [ 5. Implement ] ──> [ 6. Verify ] ──> [ 7. Complete ]
```

1. **Propose:** Define high-level product intent in `docs/PRODUCT.md`.
2. **Specify:** Draft precise specification in `docs/specs/<domain>/<feature>.md` including requirement IDs, business rules, and acceptance criteria.
3. **Review:** Peer or architectural review of spec and OpenAPI schemas.
4. **Approve:** Move spec status to `Approved`.
5. **Implement:** Write Go code strictly matching approved spec.
6. **Verify:** Execute automated tests and verify criteria against traceability matrix.
7. **Complete:** Merge implementation and update status in `docs/TRACEABILITY.md`.

---

## 2. Edge Case & Contradiction Resolution Protocol
- **Ambiguity:** If an edge case is encountered during coding, the developer/agent MUST NOT guess. The spec must be updated and approved first.
- **Conflict:** Specifications take precedence over code. Code must be altered to match specs unless a spec revision is approved.

---

## 3. Definition of Ready (DoR)
A feature is Ready for Implementation when:
- It has an approved specification with stable Requirement IDs.
- OpenAPI contract endpoint definitions are complete.
- Database constraints and schema impact are documented.
- Acceptance criteria are testable and explicit.

---

## 4. Definition of Done (DoD)
A feature is Done when:
- All acceptance criteria pass in automated tests.
- Code conforms to `docs/ENGINEERING_STANDARDS.md`.
- OpenAPI specification matches live response structures.
- `docs/TRACEABILITY.md` is updated with test evidence.

---

## 5. Reusable AI Agent Prompt Template

```text
You are working on the LearnHub repository following Spec-Driven Development (SDD).
Task: Implement requirement [INSERT REQUIREMENT ID, e.g., PRG-001].
Step 1: Read `AGENTS.md` and `docs/specs/[DOMAIN]/[SPEC_FILE].md`.
Step 2: Check `api/openapi/openapi.yaml` for expected request/response contracts.
Step 3: Implement domain logic, handlers, and persistence layer in Go.
Step 4: Write unit and integration tests verifying all acceptance criteria (AC-[ID]).
Step 5: Report completion with test evidence. Do not add unapproved features.
```
