# Database Migrations Baseline

This directory contains reference SQL scripts representing the PostgreSQL database schema for LearnHub.

> ⚠️ Note: Migrations are documented baseline designs and have not yet been executed in production.

---

## 📄 Schema Baseline (`000001_init_schema.sql`)

```sql
CREATE TYPE publication_status AS ENUM ('draft', 'published', 'archived');

CREATE TABLE tutorials (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    slug VARCHAR(255) NOT NULL UNIQUE,
    title VARCHAR(255) NOT NULL,
    summary TEXT NOT NULL,
    status publication_status NOT NULL DEFAULT 'draft',
    published_at TIMESTAMPTZ NULL,
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

CREATE TABLE lessons (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tutorial_id UUID NOT NULL REFERENCES tutorials(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    position INT NOT NULL CHECK (position > 0),
    status publication_status NOT NULL DEFAULT 'draft',
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    CONSTRAINT uq_tutorial_position UNIQUE (tutorial_id, position)
);

CREATE TABLE lesson_completions (
    learner_id UUID NOT NULL,
    lesson_id UUID NOT NULL REFERENCES lessons(id) ON DELETE CASCADE,
    completed_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    PRIMARY KEY (learner_id, lesson_id)
);

-- Indexes for efficient querying
CREATE INDEX idx_tutorials_published ON tutorials(status, published_at) WHERE status = 'published';
CREATE INDEX idx_lessons_tutorial_status ON lessons(tutorial_id, status, position);
CREATE INDEX idx_completions_learner ON lesson_completions(learner_id);
```

---

## 🔒 Invariant Enforcement via Database Constraints

1. **Unique Slugs:** `tutorials.slug` has a `UNIQUE` constraint preventing duplicate URL routes.
2. **Deterministic Lesson Position:** `CONSTRAINT uq_tutorial_position UNIQUE (tutorial_id, position)` prevents two lessons in the same tutorial from sharing the exact same sequence number.
3. **Idempotent Completion Storage:** `PRIMARY KEY (learner_id, lesson_id)` ensures a learner cannot have duplicate completion rows for the same lesson.
