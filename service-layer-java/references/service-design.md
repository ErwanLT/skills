# Service Layer Design Principles

## Goal

The Service layer implements **business use cases** and orchestrates application logic between controllers and repositories.

It must remain independent from:
- HTTP / web concerns
- persistence implementation details

---

## Core Principles

### 1. Use-case oriented design
- Each service method represents a **business action**
- Avoid generic CRUD methods like:
    - `save()`
    - `update()`
    - `process()`

Prefer:
- `createBook()`
- `assignAuthorToBook()`
- `archiveUserAccount()`

---

### 2. Business logic centralization
- All business rules must be in services
- Controllers must remain thin
- Repositories must remain data-focused

---

### 3. Explicit orchestration
- Services can coordinate multiple repositories
- Services can call other services
- Avoid hidden side effects

---

### 4. Stateless design
- Services must not store state between requests
- No request-scoped business state in fields

---

## Anti-patterns

- Fat Controller (logic in controllers)
- Anemic Service (only delegation, no logic)
- God Service (too many responsibilities)
- Business logic in repositories
- Hidden rules in entities

---

## Design Guidance

- Prefer composition over inheritance
- Split services when responsibilities diverge
- Keep methods small and intention-revealing