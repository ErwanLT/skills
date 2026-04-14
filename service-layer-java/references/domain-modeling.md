# Domain Modeling Guidelines

## Goal

Ensure the service layer reflects real business concepts, not technical constructs.

---

## Core Principles

### 1. Business-first thinking
- Model services around business capabilities
- Avoid technical naming in service methods

---

### 2. Explicit domain rules
- Business rules must be visible in service logic
- Avoid hiding rules in:
    - repositories
    - entities
    - mappers

---

### 3. Clear boundaries
- Each service should represent a **cohesive domain area**
- Avoid mixing unrelated business concerns

---

## Naming guidelines

Prefer:
- domain-oriented verbs
- business language

Avoid:
- generic technical terms (`handle`, `process`, `execute`)

---

## Service cohesion

A service should answer:
> “What business capability does this represent?”

If the answer is unclear → split the service.