# Tutorials Domain Boundary

## Responsibilities
- Managing tutorial metadata (title, summary, slug, publication status).
- Providing listing of published tutorials for public consumption.
- Validating tutorial existence by slug.

## Key Invariants
- Tutorials in `draft` or `archived` status must never be returned to public API consumers.
- Slugs are unique and immutable once published.
