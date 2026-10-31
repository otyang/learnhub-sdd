# LearnHub — Spec-Driven Development (SDD) Starter Repository

Welcome to the **LearnHub** starter repository! LearnHub is a simple online learning platform designed to demonstrate **Spec-Driven Development (SDD)** with AI coding agents in Go.

> ⚠️ **Note:** This repository is an SDD documentation starter. It contains baseline specifications, domain boundaries, architecture decision records (ADRs), an OpenAPI contract, and database migration designs. It does **not** contain the completed Go application code.

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

## 📖 Spec-Driven Development Workflow

In this project, **no application code is written without an approved specification**.

1. Read [`docs/WORKFLOW.md`](docs/WORKFLOW.md) to understand the full specification-first lifecycle.
2. Review [`docs/TRACEABILITY.md`](docs/TRACEABILITY.md) to see how product requirements map to feature specifications and test cases.
3. Consult [`AGENTS.md`](AGENTS.md) if you are an AI agent implementing feature requests.

---

## 🤖 Guidance for AI Coding Agents

If you are an AI assistant tasked with writing code for this repository:
1. Read **`AGENTS.md`** first.
2. Verify that the task maps to an approved feature specification in `docs/specs/`.
3. Follow `docs/ENGINEERING_STANDARDS.md` and `docs/ARCHITECTURE.md`.
4. Ensure code implementation satisfies all acceptance criteria in the spec without introducing unapproved features.
