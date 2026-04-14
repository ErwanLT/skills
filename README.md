# Skills

A structured collection of AI skills for Java backend development.

This repository defines a **modular agent system** focused on clean architecture, separation of concerns, and production-grade Java development practices.

Each skill targets a specific layer of the application architecture.

---

## Available Skills

### [Dev App Java](./dev-app-java/SKILL.md)

Cross-cutting Java application development and quality improvement (structure, refactoring, tests, documentation) for Vanilla Java, Spring Boot, Quarkus, or Struts.

Focus:
- Code quality
- Design patterns
- Testing strategy
- Java 21 best practices

---

### [API Design & Documentation](./api-design/SKILL.md)

Design and implementation of RESTful APIs with a strong focus on controller responsibilities.

Focus:
- REST design principles
- DTO usage
- OpenAPI documentation
- HTTP correctness
- Thin controllers (no business logic)

---

### [Service Layer (Business Logic](./service-layer-java/SKILL.md)

Design and orchestration of business use cases and transactional logic.

Focus:
- Business use-case design
- Transactional boundaries
- Service orchestration
- Separation from API and persistence layers
- Clean domain logic

---

### [DB Persistence & SQL](./db-persistence-sql/SKILL.md)

Design and optimization of the persistence layer using PostgreSQL, JPA/Hibernate, and migration tools.

Focus:
- Repository design
- Query performance
- ORM best practices
- Schema evolution (Liquibase/Flyway)
- Data integrity and modeling

---

## Architecture Overview

This system is designed around a classic layered architecture:

- **API Layer** → Request handling and HTTP contracts
- **Service Layer** → Business logic and use cases
- **Persistence Layer** → Data access and storage
- **Cross-cutting Layer** → Java quality, testing, and design best practices

---

## Design Principles

- Strict separation of concerns
- Use-case oriented design (not CRUD-oriented)
- Explicit and readable code over abstraction
- Avoid over-engineering
- Keep business logic in the service layer

---

## Agent Integration

These skills are designed to be used by a model-driven agent system.

Routing is handled via:
- `agent.yaml` (skill selection and priority)
- skill triggers (context detection)
- interface prompts (user entry points)

---

## Philosophy

This project follows a traditional software engineering approach:

- Clear layering
- Explicit responsibilities
- Stable and maintainable architecture
- Minimal complexity, maximum clarity
