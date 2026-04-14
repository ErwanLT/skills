# Hibernate & JPA Best Practices

## Table of Contents
- Mapping Essentials
- Solving the N+1 Problem
- Projections (DTOs vs Entities)
- Transaction Management
- Batch Processing
- Review Checklist

---

## Mapping Essentials

- **Lazy by Default**: Always use `FetchType.LAZY` for `@OneToMany`, `@ManyToMany`, and `@ManyToOne`.
- **Bidirectional Mappings**: Ensure you maintain both sides of the relationship correctly in your Java code (using helper methods).
- **Identifier Generation**: Use `GenerationType.SEQUENCE` for performance (avoid `IDENTITY` if possible as it disables JDBC batching).

---

## Solving the N+1 Problem

- **JOIN FETCH**: Use `JOIN FETCH` in JPQL or HQL to retrieve associations in a single query.
- **EntityGraph**: Use `@EntityGraph` for a more declarative way to define fetching strategies for specific queries.
- **Batch Size**: Configure `hibernate.jdbc.batch_size` and use `@BatchSize(size = ...)` to fetch collections in chunks if `JOIN FETCH` is not appropriate.

---

## Projections (DTOs vs Entities)

- **Entities**: Use only for **updates** or when the persistence context management is required.
- **DTO Projections**: Use for **read-only** operations to reduce memory footprint and avoid "LazyInitializationException".
  ```java
  @Query("SELECT new com.example.UserDTO(u.name, u.email) FROM User u")
  List<UserDTO> findAllProjected();
  ```
- **Interface-based Projections**: Use Spring Data JPA interfaces for lightweight projections.

---

## Transaction Management

- **Read-Only**: Use `@Transactional(readOnly = true)` for pure reads to optimize performance and memory.
- **Boundary**: Keep transactions at the **Service** layer.
- **Flush Mode**: Hibernate's default `AUTO` flush is usually safe, but be aware of when changes are sent to the DB.

---

## Batch Processing

- Use `StatelessSession` for high-volume data processing to bypass the persistent context (1st level cache).
- Regularly `clear()` the session if processing many entities in a single transaction to prevent memory issues.

---

## Review Checklist

- [ ] Associations are `LAZY`.
- [ ] No N+1 queries in the logs (use SQL logging to verify).
- [ ] DTOs used for high-volume read operations.
- [ ] Transactions are at the Service level.
- [ ] `@Transactional(readOnly = true)` used where appropriate.
- [ ] Primary keys use `SEQUENCE` for optimal batching.
