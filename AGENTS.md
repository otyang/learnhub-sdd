# AGENTS.md — Instructions for AI Coding Agents

This repository follows **Spec-Driven Development (SDD)**. As an AI coding agent working on LearnHub, you must strictly follow the rules below.

---

## 🛡️ Core Rules & Operating Principles

1. **Specification First:** Never write production code, change APIs, or alter database schemas without an approved specification in `docs/specs/` or an approved ADR in `docs/decisions/`.
2. **No Unapproved Requirements:** Do not invent features, business logic, or extra security layers (e.g., do not add login/passwords, payment gateways, or user accounts). Stick strictly to what is documented.
3. **Traceability:** Every pull request or code change must reference stable requirement IDs (e.g., `CAT-001`, `LES-002`, `PRG-003`).
4. **No Premature Assumptions:** If a specification is ambiguous or missing edge-case handling, **ask the user for clarification** or propose an update to the spec before writing code. Do not silently guess business behavior.

---

## 📋 Required Pre-Flight Checklist

Before writing any Go code, you **must**:
1. Read `docs/PRODUCT.md` for domain context.
2. Read `docs/ARCHITECTURE.md` to understand package boundaries (`internal/` vs `pkg/`).
3. Read `docs/ENGINEERING_STANDARDS.md` for Go idioms, HTTP formatting, and error response standards.
4. Read the target spec in `docs/specs/<domain>/<feature>.md`.
5. Review the corresponding OpenAPI 3.1 contract in `api/openapi/openapi.yaml`.

---

## ⚙️ Execution Flow for Implementation Tasks

When assigned an implementation task:
1. **Locate Spec & Requirements:** Identify the relevant Spec ID and Requirement IDs.
2. **Review Tests & Schema:** Check existing tests in `tests/` and database migrations in `migrations/`.
3. **Implement Business Invariants:** Implement domain models and database interactions without violating invariants (e.g., unpublished tutorials return 404, progress calculation formula).
4. **Update Artefacts:** If your implementation alters APIs or schemas, ensure `api/openapi/openapi.yaml` and `docs/TRACEABILITY.md` are updated simultaneously.
5. **Verify:** Run tests (`go test ./...`) and linting before marking the task complete.

---

## 🔒 Anonymous Learner Identification Note

Notice that `X-Learner-ID` is used for progress tracking only. **Do not** treat `X-Learner-ID` as an authentication token or security mechanism. Do not implement JWTs, session tokens, or password hashes unless explicitly instructed via a new approved specification.
