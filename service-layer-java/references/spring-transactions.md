# Transaction Management (Spring Boot)

## Goal

Ensure data consistency and correct transactional boundaries in service layer.

---

## Core Rules

### 1. Service layer responsibility
- Transactions must be defined in services only
- Never use `@Transactional` in controllers or repositories

---

### 2. Transaction types

- `@Transactional`
  → write operations

- `@Transactional(readOnly = true)`
  → read operations

---

### 3. Transaction boundaries
- Keep transactions as short as possible
- Do not include external API calls inside transactions

---

## Common Mistakes

- Starting transactions in controllers
- Long-running transactions
- Mixing I/O (HTTP calls, messaging) inside transactions

---

## Performance considerations

- Avoid holding locks longer than necessary
- Prefer batch operations when possible
- Be cautious with lazy loading inside transactions