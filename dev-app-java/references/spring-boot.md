# Spring Boot Best Practices (Tests, Patterns)

## Table of Contents
- Testing
- Architecture & Layers
- Common Patterns
- Transactions & Persistence
- API & OpenAPI
- Review Checklist

---

## Testing

- Test layers in isolation:
  - `@WebMvcTest` for MVC controllers
  - `@DataJpaTest` for the JPA layer
  - Pure unit tests for services

- Use `@SpringBootTest` only for full integration tests
- Avoid making it the default choice (high cost)

- Use **Testcontainers** for external dependencies (DB, Kafka)
- Configure dedicated profiles (`@ActiveProfiles("test")`)

- Clearly separate:
  - unit tests (fast)
  - integration tests (slower)

- Structure in **Given / When / Then**
- Test behavior, not implementation

**When to act:**
- Tests are too slow or unstable
- Excessive use of `@SpringBootTest`
- Low service coverage

---

## Architecture & Layers

- Clearly separate:
  - **Controller** → HTTP exposure
  - **Service** → business logic
  - **Repository** → data access

- Controllers should remain **thin**
- Business logic belongs in services
- Repositories should only contain data access logic

- Do not mix business and technical logic

- Favor coherent services (one clear responsibility)

**When to act:**
- Business logic in controllers
- Overly large services
- Strong coupling between layers

---

## Common Patterns

- **Constructor** injection (recommended)
  - Promotes immutability and testability

- Use **DTOs** for API exposure
- Explicitly map DTO ↔ domain

- Prefer composition over inheritance

- Common patterns:
  - **Strategy**: business variants
  - **Factory**: complex creation
  - **Adapter**: external integration

- Use `@Configuration` to centralize configurations

**When to act:**
- Code is difficult to test
- Logic duplication
- Coupling with external APIs

---

## Transactions & Persistence

- Manage transactions at the **service** level (`@Transactional`)
- Never manage transactions in a controller

- Be explicit about:
  - read-only (`readOnly = true`)
  - propagation if necessary

- Avoid:
  - business logic in entities
  - direct repository access from the controller

- Monitor:
  - lazy loading
  - N+1 issues

**When to act:**
- Transaction-related bugs
- Degraded database performance
- Poorly controlled data access

---

## API & OpenAPI

- Systematically document with OpenAPI:
  - `@Operation`
  - `@ApiResponse`
  - `@Parameter`

- Document:
  - nominal case
  - error cases

- Do not directly expose JPA entities
- Use dedicated DTOs

- Standardize errors (e.g., `ProblemDetail`)

**When to act:**
- API is difficult to understand
- Implicit contracts
- Documentation is missing

---

## Review Checklist

### Tests
- Unit tests present for services
- `@SpringBootTest` usage is justified
- Layered tests (Web / JPA / Service)

### Architecture
- Thin controllers
- Well-partitioned services
- Isolated repositories

### Code
- Constructor injection
- Explicit naming
- Readable methods

### Persistence
- Transactions at the right level
- No business logic in entities
- No direct DB access from controllers

### API
- Complete OpenAPI
- DTOs used
- Documented errors

### Global
- Code quickly understandable
- Clear responsibilities
- No unnecessary complexity

---

## Example (OpenAPI on Spring Boot)

```java
@GetMapping
@Operation(summary = "Retrieve all books",
        description = "Retrieves the full list of books")
@ApiResponses({
        @ApiResponse(responseCode = "200",
                description = "List of books successfully retrieved",
                content = @Content(mediaType = "application/json",
                        array = @ArraySchema(schema = @Schema(implementation = Book.class)))),
        @ApiResponse(responseCode = "404",
                description = "No books found",
                content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = ProblemDetail.class))),
        @ApiResponse(responseCode = "500",
                description = "Error during processing",
                content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = ProblemDetail.class)))
})
public List<Book> getBooks() throws BookException {
    return bookService.findAllBooks();
}
```
