# Database Migrations (Liquibase & Flyway)

## Table of Contents
- Migration Workflow
- Liquibase Best Practices
- Flyway Best Practices
- Safety & Performance
- Review Checklist

---

## Migration Workflow

1. **One Change, One File**: Create a separate file (or changeset) for each logical modification.
2. **Immutable Files**: Once a migration is pushed to a shared branch/environment, NEVER modify it. Add a new migration to correct it.
3. **Idempotency**: Migrations must be repeatable and check for existing states if possible.

---

## Liquibase Best Practices

- Use **YAML, JSON, or XML** formats over SQL for better abstraction and automatic rollbacks.
- Use **Contexts** and **Labels** to separate test data from schema changes.
- Always include a **Rollback** section for custom SQL changes.
- Use `preConditions` to prevent errors (e.g., "table already exists").

---

## Flyway Best Practices

- **Naming Convention**: Follow `V<Version>__Description.sql` strictly.
- **Repeatable Migrations**: Use `R__Description.sql` for objects like Views, Procedures, or Triggers.
- **Baseline**: Use the baseline feature when introducing Flyway to an existing database.

---

## Safety & Performance

- **Avoid Long Locks**: Be careful with `ALTER TABLE` on large tables (use `CREATE INDEX CONCURRENTLY` in Postgres).
- **Default Values**: Adding a `NOT NULL` column with a default value can be slow on large tables in older Postgres versions (PG 11+ is safe).
- **Data Migration**: Separate schema changes (DDL) from data migrations (DML). Run DML in batches for large volumes.

---

## Review Checklist

- [ ] Migration files follow the project's naming convention.
- [ ] Existing migration files have not been modified.
- [ ] Migrations are idempotent.
- [ ] Critical production changes (e.g., index creation) are non-blocking where possible.
- [ ] Rollback plan is verified.
