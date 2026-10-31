# LearnHub — AI-Driven Spec-Driven Development (SDD) Starter

**LearnHub** is a starter repository for **AI-assisted Spec-Driven Development**: humans write and approve the specifications, and **AI coding agents** (Claude Code, Codex, Copilot, Cursor and similar) implement them in Go.

LearnHub itself is a deliberately small online learning platform. The real subject is the workflow: the repository is set up so an AI agent can take a requirement ID, find its approved spec and API contract, and build exactly what is specified, with nothing invented along the way.

> ⚠️ **Note:** This repository is an SDD documentation starter. It contains baseline specifications, domain boundaries, architecture decision records (ADRs), an OpenAPI contract, database migration designs, and AI agent instructions. It does **not** contain the completed Go application code. Writing that code is the AI agent's job.

---

## 🤖 Why AI + SDD?

AI coding agents are fast, but left unguided they guess at business rules, add features nobody asked for, and drift from the API contract. This repository constrains them with specifications:

| Problem with unguided AI agents | How this repo constrains them |
| --- | --- |
| Invents requirements or "helpful" extras | [`AGENTS.md`](AGENTS.md) forbids unapproved features; only approved specs in `docs/specs/` may be implemented |
| Guesses on ambiguous edge cases | Agents must stop and ask, or propose a spec update, before coding |
| Drifts from the API contract | [`api/openapi/openapi.yaml`](api/openapi/openapi.yaml) is the source of truth for every request and response |
| Produces changes that are hard to review | Every change cites stable requirement IDs (`CAT-001`, `LES-002`, `PRG-003`), traced in [`docs/TRACEABILITY.md`](docs/TRACEABILITY.md) |
| Makes architectural calls on its own | Decisions are fixed in ADRs under [`docs/decisions/`](docs/decisions/) |

---

## 🧠 SDD, Not Vibe Coding

This is **AI-assisted, spec-driven development**, not vibe coding.

**Vibe coding** means prompting an AI loosely, accepting whatever it writes if it seems to work, and iterating by feel. That works for throwaway prototypes, but the AI fills every gap in the prompt with its own assumptions, and nobody can say afterwards which behaviour was intended and which was guessed.

**In this repository, the AI implements decisions that humans have already made:**

- **Intent is written down first.** Each behaviour has a requirement ID, business rules and acceptance criteria in `docs/specs/`, so the agent builds what was decided rather than what it infers.
- **Rules are fixed, not improvised.** Invariants like "unpublished tutorials return 404" and the progress-percentage formula ([ADR-003](docs/decisions/ADR-003-progress-calculation.md)) are set before any code exists. The AI cannot quietly change them.
- **Scope is enforced.** [ADR-001](docs/decisions/ADR-001-no-user-accounts.md) rules out user accounts. Without it, an AI might add logins or JWTs because they look like good practice. Here, that counts as a defect.
- **Ambiguity stops the work.** If a spec is unclear, the agent asks or proposes a spec change; it does not guess.
- **Results can be verified.** Every change traces back to requirement IDs and acceptance tests in [`docs/TRACEABILITY.md`](docs/TRACEABILITY.md), so reviewers check the work against the spec, not against a feeling that it works.

The AI still does most of the typing. What changes is accountability: humans own *what* gets built and *why*, the AI owns *how*, and the spec is the contract between them.

---

## 🚀 Project Overview

LearnHub enables anonymous learners to:
1. Browse published tutorials.
2. View ordered lessons within a tutorial.
3. Read lesson contents with prev/next navigation.
4. Track completed lessons and view progress percentage (stored per anonymous browser UUID via `X-Learner-ID`).

---

## 🏗️ Technology & Baseline Architecture

- **Language:** Go 1.22+
- **Architecture:** Modular Monolith (`cmd/all`)
- **Database:** PostgreSQL
- **API Contract:** OpenAPI 3.1 (HTTP JSON API)
- **Frontend:** Single Page Application (SPA) / static frontend (separate context)

---

## 📁 Repository Structure

```text
learnhub-sdd/
├── AGENTS.md                  # Crucial instructions for AI coding agents
├── README.md                  # Project overview and SDD entry point
├── go.mod                     # Go module definition
├── .gitignore
├── .env.example
├── cmd/all/                   # Single executable composition root
├── internal/                  # Domain modules (tutorials, lessons, progress)
├── pkg/                       # Shared utility packages (config, database, httpserver, id)
├── api/openapi/               # OpenAPI 3.1 specification
├── migrations/                # Database SQL migration scripts baseline
├── web/                       # Frontend application workspace placeholder
├── tests/                     # System & integration test blueprints
└── docs/                      # Core SDD specs, ADRs, engineering standards, and traceability
```

---

## 📖 The AI-Driven SDD Workflow

In this project, **no application code is written, by a human or an AI, without an approved specification**.

```text
 Human                                   AI agent                          Human
[ Propose → Specify → Review → Approve ] → [ Implement → Test → Trace ] → [ Verify → Merge ]
```

- **Humans own intent:** product goals, specs, acceptance criteria, ADRs, and approval.
- **AI agents own implementation:** Go code, tests, and traceability updates that match the approved spec.
- **Specs settle disputes:** if code and spec disagree, the code changes, unless a spec revision is approved.

See [`docs/WORKFLOW.md`](docs/WORKFLOW.md) for the full lifecycle, Definition of Ready and Definition of Done.

---

## 🚦 Getting Started with an AI Agent

1. Pick a requirement ID from [`docs/TRACEABILITY.md`](docs/TRACEABILITY.md).
2. Point your AI coding agent at this repository. It should read [`AGENTS.md`](AGENTS.md) first; many agents pick it up automatically.
3. Give it the reusable prompt from [`docs/WORKFLOW.md`](docs/WORKFLOW.md#5-reusable-ai-agent-prompt-template):

   ```text
   You are working on the LearnHub repository following Spec-Driven Development (SDD).
   Task: Implement requirement PRG-001.
   Step 1: Read `AGENTS.md` and `docs/specs/progress/lesson-progress.md`.
   Step 2: Check `api/openapi/openapi.yaml` for expected request/response contracts.
   Step 3: Implement domain logic, handlers, and persistence layer in Go.
   Step 4: Write unit and integration tests verifying all acceptance criteria (AC-[ID]).
   Step 5: Report completion with test evidence. Do not add unapproved features.
   ```

4. Review the agent's change against the spec's acceptance criteria and the traceability matrix before merging.

---

## 🧭 Rules for AI Coding Agents

If you are an AI assistant tasked with writing code for this repository:
1. Read **[`AGENTS.md`](AGENTS.md)** first. Its rules override your defaults.
2. Verify that the task maps to an approved feature specification in `docs/specs/`.
3. Follow `docs/ENGINEERING_STANDARDS.md` and `docs/ARCHITECTURE.md`.
4. Satisfy every acceptance criterion in the spec without introducing unapproved features.
5. If the spec is ambiguous, **ask; do not guess.**

---

## 👥 Contributors

- [Otyang](https://github.com/otyang)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
