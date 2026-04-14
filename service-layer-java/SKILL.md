---
name: service-layer-java
description: "Design and implementation of the Service layer in Java applications (Spring Boot, Quarkus, Vanilla Java). Focus on business logic orchestration, use-case design, transactional boundaries, and clean separation between API and persistence layers."
---

# Service Layer Design
## Overview

The Service layer is responsible for business logic orchestration and use-case execution.

It acts as the central layer between:

- Controllers (API layer)
- Repositories (persistence layer)

It must remain independent of HTTP and persistence concerns.

## Core Responsibilities
- Implement business use cases, not CRUD operations
- Orchestrate multiple repositories or external services
- Enforce business rules and invariants
- Define transactional boundaries
- Provide a clean API for the controller layer

## Workflow
### Identify the Use Case
- Map each service method to a business action 
- Avoid generic methods like `save()`, `update()`, `process()`
### Apply Business Logic
- Centralize domain rules inside services 
- Avoid delegating logic to controllers or repositories 
- Keep logic explicit and readable
### Manage Transactions
- Define transaction boundaries at service level
- Use `@Transactional` only in services
- Keep transactions as short as possible
### Coordinate Dependencies
- Use repositories for data access only 
- Use other services for cross-domain logic 
- Avoid circular dependencies

## Core Principles
### Use-case oriented design
- Each method represents a business intent
- Method names must express domain actions
### Separation of concerns
- Controller → HTTP handling only
- Service → business logic only
- Repository → data access only
### Stateless services
Services must not hold state between calls
### Explicit logic
- Prefer explicit code over hidden abstractions
- Avoid over-engineering and unnecessary patterns

## Transactions
Always define transactions at service level

Use:
- `@Transactional` for write operations
- `@Transactional(readOnly = true)` for read operations

Avoid transactional logic in controllers or repositories

## Anti-patterns
- Fat Controller (business logic in controllers)
- Anemic Service (services without real logic)
- God Service (too many responsibilities)
- Direct repository usage in controllers 
- Hidden business rules inside entities

## Design Guidelines
- Prefer composition over inheritance
- Keep services small and focused
- Split services when responsibilities diverge
- Use domain-driven method naming

## Example
```java
@Service
public class BookService {

    private final BookRepository bookRepository;

    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    @Transactional
    public BookDto createBook(CreateBookCommand command) {
        validate(command);

        Book book = new Book(command.title(), command.author());

        Book saved = bookRepository.save(book);

        return BookDto.from(saved);
    }

    @Transactional(readOnly = true)
    public List<BookDto> getAllBooks() {
        return bookRepository.findAll()
                .stream()
                .map(BookDto::from)
                .toList();
    }

    private void validate(CreateBookCommand command) {
        if (command.title() == null || command.title().isBlank()) {
            throw new IllegalArgumentException("Title is required");
        }
    }
}
```

## Review Checklist
### Architecture
- Methods correspond to use cases
- No business logic in controllers
- No repository leakage outside services
### Design
- Clear responsibilities per service
- No god services
- No anemic services
### Transactions
- Correct transactional boundaries
- No transactions outside services
### Code quality
- Readable business logic
- Explicit naming
- Minimal boilerplate

## References
- For transaction management, read `references/spring-transactions.md`
- For service design principles, read `references/service-design.md`
- For domain modeling, read `references/domain-modeling.md`