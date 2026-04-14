---
name: db-persistence-sql
description: "Design and optimization of the persistence layer (PostgreSQL, JPA/Hibernate, Migrations). Focus on the 'Repository' part of the Controller/Service/Repository pattern, ensuring data integrity, high performance, and clean migrations."
---

# DB Persistence & SQL

## Overview

Ensure the data layer is robust, performant, and maintainable. This skill focuses on the bridge between Java and PostgreSQL, optimizing Hibernate/JPA, and managing database evolution via migrations.

## Workflow

1. **Analyze the Schema**
   - Review the database schema: types, constraints (FK, Unique, Not Null), and indexes.
   - For existing tables, check for missing indexes on frequently queried columns.

2. **Optimize the Repository Layer**
   - Identify potential N+1 query problems in JPA mappings.
   - Use **DTO Projections** instead of full Entities for read-only operations.
   - Leverage native SQL for complex reporting or batch operations.

3. **Manage Evolution (Migrations)**
   - Use **Liquibase** or **Flyway** for all schema changes.
   - Ensure migrations are idempotent and can be safely applied in CI/CD.

4. **Validate Performance**
   - Use `EXPLAIN ANALYZE` on complex queries.
   - Check connection pool settings (HikariCP) and transaction boundaries.

## Core Principles

### PostgreSQL & SQL
- Use appropriate types: `JSONB` for unstructured data, `TIMESTAMPTZ` for dates, `UUID` for identifiers.
- Create indexes on foreign keys and frequently filtered columns.
- Prefer explicit joins over implicit ones.

### Hibernate & JPA
- **Default to Lazy Loading**: Never use `EAGER` fetching (it's the N+1 trap).
- **Fetch Joins**: Use `JOIN FETCH` or `EntityGraph` only when the data is truly needed.
- **Stateless Session**: For batch processing, consider `StatelessSession` to avoid the overhead of the persistence context.

### Database Migrations
- One migration file per change.
- Never modify an existing migration file (add a new one to correct/revert).
- Include rollbacks where possible.

## References

- For PostgreSQL specific rules and optimization, read `references/postgresql.md`.
- For Hibernate and JPA best practices, read `references/hibernate-jpa.md`.
- For Liquibase and Flyway management, read `references/migrations.md`.
