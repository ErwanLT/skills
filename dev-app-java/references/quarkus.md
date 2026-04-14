# Quarkus Best Practices (Tests, Patterns)

## Table of Contents
- Testing
- Architecture & Layers
- Common Patterns
- API & OpenAPI
- Review Checklist

---

## Testing

- Use `@QuarkusTest` for integration tests.
- Limit its usage: starting the context has a non-negligible cost.
- Write **pure unit tests** without Quarkus as much as possible.
- Use `@QuarkusTestResource` to manage external dependencies (DB, services).
- Clearly separate:
  - fast tests (unit)
  - tests with context (integration)

- Structure tests in **Given / When / Then**
- Test behavior, not implementation

**When to act:**
- Tests are too slow
- Low coverage outside of integration
- Excessive dependency on the Quarkus runtime

---

## Architecture & Layers

- Clearly separate:
  - **Resource (API)** → HTTP exposure
  - **Service** → business logic
  - **Repository** → data access

- Endpoints should remain **thin**
- All business logic should be in services
- Do not let technical logic leak into the domain

- Favor **coherent and targeted** services
- Avoid "catch-all" classes

**When to act:**
- Business logic in resources
- Overly large or ambiguous services
- Strong coupling between layers

---

## Common Patterns

- Injection via `@Inject`
  - Allow constructors to improve testability

- Scopes:
  - `@ApplicationScoped` for services
  - `@Singleton` if purely stateless

- Use **DTOs** for external exposure
- Explicitly map DTO ↔ domain

- Prefer composition over inheritance

- Common patterns:
  - **Strategy**: business variants
  - **Factory**: complex instantiation
  - **Adapter**: external integration

**When to act:**
- Logic duplication
- Strong coupling with external APIs
- Difficulty evolving the code

---

## API & OpenAPI

- Systematically document with OpenAPI:
  - `@Operation`
  - `@APIResponse`
  - `@Parameter`

- Always document:
  - nominal case
  - possible errors

- Use dedicated objects for responses (DTO)
- Never directly expose internal entities

- Standardize errors (e.g., `ProblemDetail`)

**When to act:**
- API is difficult to understand
- Implicit contracts
- Documentation is missing or incomplete

---

## Review Checklist

### Tests
- Unit tests present without starting Quarkus
- `@QuarkusTest` usage is justified
- Edge cases and errors covered

### Architecture
- Thin Resource, no business logic
- Well-partitioned services
- Isolated data access

### Code
- Explicit naming
- Short and readable methods
- No obvious duplication

### API
- Complete OpenAPI
- DTOs used
- Documented errors

### Global
- Code quickly understandable
- Clear responsibilities
- No unnecessary complexity

---

## Example (OpenAPI on Quarkus)

```java
@GET
@Path("/greeting")
@Operation(summary = "Greet an adventurer",
        description = "Returns a welcome message from the innkeeper.")
@APIResponse(
        responseCode = "200",
        description = "Welcome message",
        content = @Content(schema = @Schema(implementation = TavernGreetingResponse.class))
)
@APIResponse(
        responseCode = "400",
        description = "Invalid parameter",
        content = @Content(schema = @Schema(implementation = ProblemDetail.class))
)
public TavernGreetingResponse greeting(
        @Parameter(description = "Name of the adventurer to greet", example = "Arthas")
        @QueryParam("name") String name
) {
    return tavernService.greeting(name);
}
```
