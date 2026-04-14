# Java Best Practices (Javadoc, Tests, Patterns)

## Table of Contents
- Javadoc Rules
- Unit Testing Rules
- Design Pattern Guidance
- Review Checklist
- Example Requests

## Javadoc Rules

- Document the functional contract: what the method does, not how.
- Describe parameters with constraints (nullability, formats, units).
- Describe the return value (business meaning, sentinel values).
- List exceptions and trigger conditions.
- Add a usage example if the method is non-trivial.
- Avoid repetition with the method name; explain side effects.

Minimal example:
```java
/**
 * Calculates the total including tax from an amount excluding tax.
 * @param amountExTax amount excluding tax, in euros, strictly positive
 * @param vatRate VAT rate between 0 and 1
 * @return total including tax rounded to 2 decimal places
 * @throws IllegalArgumentException if amountExTax <= 0 or vatRate is out of bounds
 */
```

## Unit Testing Rules

- Use JUnit 5 (`@Test`, `@BeforeEach`), Mockito if necessary.
- Recommended structure: Arrange / Act / Assert.
- Cases to cover:
  - Nominal (happy path)
  - Edge cases (boundaries, formats, nulls)
  - Errors (expected exceptions)
  - Invariants (properties that must always hold)
- Avoid:
  - Network/file access in unit tests
  - Global static dependencies
  - Weak assertions (e.g., `assertTrue(obj != null)`)

## Design Pattern Guidance

- Do not introduce a pattern if a simple solution is clear and stable.
- Typical indications:
  - Multiple conditional branches based on a type or rule: Strategy.
  - Construction of complex objects or product variants: Factory/Builder.
  - Adaptation of a third-party API: Adapter.
  - Dynamic addition of behavior: Decorator.
- Check the impact on readability and maintenance cost.

## Review Checklist

- Javadoc
  - Explicit and up-to-date contract
  - Exceptions documented
  - Clear parameters/return
- Tests
  - Edge cases covered
  - Meaningful assertions
  - No external dependencies
- Design
  - Consistent responsibilities
  - Managed complexity
  - Pattern justified by a real need

## Example Requests

- "Add missing Javadoc to these public methods"
- "Propose unit tests for this class"
- "Refactor this block using an appropriate pattern"
