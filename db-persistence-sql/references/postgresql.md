# PostgreSQL Best Practices

## Table of Contents
- Data Types & Constraints
- Indexing Strategy
- Query Performance (EXPLAIN)
- Advanced Features (JSONB, Full Text Search)
- Review Checklist

---

## Data Types & Constraints

- **IDs**: Use `UUID` or `BIGINT IDENTITY` for primary keys. Avoid simple `INT`.
- **Dates**: Always use `TIMESTAMPTZ` to avoid timezone confusion in the application layer.
- **Numbers**: Use `DECIMAL` / `NUMERIC` for monetary values. Never use `FLOAT` or `REAL` for financial data.
- **Constraints**: Enforce business rules at the database level (`NOT NULL`, `CHECK`, `FOREIGN KEY`, `UNIQUE`).

---

## Indexing Strategy

- **Foreign Keys**: Always create an index on FK columns to speed up joins and cascading deletes.
- **Indexes**: Focus on columns used in `WHERE`, `JOIN`, `ORDER BY`, and `GROUP BY`.
- **Composite Indexes**: Use them for queries filtering on multiple columns. Order columns from most selective to least selective.
- **Partial Indexes**: Use them to index a subset of rows (e.g., `WHERE status = 'ACTIVE'`) to save space.

---

## Query Performance (EXPLAIN)

- Use `EXPLAIN (ANALYZE, BUFFERS)` to diagnose slow queries.
- Look for **Sequential Scans** (Seq Scan) on large tables; they usually indicate a missing index.
- Watch out for **Nested Loops** on large joins; consider if the data can be denormalized or if the join is inefficient.

---

## Advanced Features

- **JSONB**: Use `JSONB` for flexible data that needs to be queried. Prefer standard columns for stable, core business data.
- **Upserts**: Use `INSERT ... ON CONFLICT (id) DO UPDATE SET ...` to handle duplicates gracefully.
- **Window Functions**: Use `ROW_NUMBER()`, `RANK()`, or `OVER()` for complex analytics directly in SQL.

---

## Review Checklist

- [ ] Every table has a primary key with a robust type (`BIGINT` or `UUID`).
- [ ] Foreign keys are indexed.
- [ ] Mandatory fields have `NOT NULL` constraints.
- [ ] Dates use `TIMESTAMPTZ`.
- [ ] Financial data uses `NUMERIC`.
- [ ] No sequential scans found in critical paths.
