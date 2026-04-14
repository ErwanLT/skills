---
name: dev-app-java
description: "Cross-cutting Java application development and quality improvement (structure, refactoring, tests, documentation) for Vanilla Java, Spring Boot, Quarkus, or Struts. Use it when building or evolving a Java application with a focus on maintainability, clarity, and best practices."
---

# Dev App Java

## Overview

Apply cross-cutting Java best practices on existing or new code, with **Java 21** as the baseline version.

Focus on producing clear, maintainable, and well-structured code, favoring explicit design over implicit or overly generic solutions.

## Workflow

1. **Clarify Context**
- Identify the application type (Vanilla Java, Spring Boot, Quarkus, Struts)
- Identify the concrete goal (feature, refactoring, bug fix, review). 
- Do not assume frameworks or libraries beyond what is explicitly stated.

2. **Diagnose Needs**
- Javadoc: missing contracts, ambiguities, undocumented edge cases. 
- Tests: missing coverage (edge cases, errors), weak or brittle assertions. 
- Design: duplicated logic, mixed responsibilities, lack of clarity.

3. **Apply Targeted Best Practices**
- Apply only necessary changes. 
- Prefer simple and explicit solutions over generic or abstract ones. 
- Avoid over-engineering; introduce patterns only when they solve a concrete problem.

4. **Validate**
- Ensure readability and consistency.
- Verify impact on existing code.
- Add or adapt unit tests if behavior changes.

## Code Readability & Domain
- Use explicit, domain-oriented naming.
- Keep business logic readable without requiring technical knowledge.
- Avoid leaking infrastructure concerns into domain logic.
- Prefer small, focused methods and classes.

## Modern Java (17/21)
- Prefer records for immutable data structures.
- Use pattern matching and enhanced switch when it improves clarity.
- Use sealed classes to model controlled hierarchies when relevant.
- Reduce boilerplate using modern language features when it improves readability.

## Javadoc
- Define a clear contract: role, preconditions, postconditions, invariants.
- Document parameters, return values, and exceptions (with conditions).
- Explain the intent ("why"), not just the behavior.
- Provide examples when usage is not obvious.
- Keep documentation synchronized with the code.

### When to act:
- Public method without explicit contract
- Implicit or surprising behavior
- Undocumented error cases

## Unit Testing
- Use JUnit 5 by default.
- Test behavior, not implementation details.
- Cover nominal, edge, and error cases.
- Avoid brittle tests (time, order, shared state).
- Use clear and descriptive test names.

### When to act:
- Missing tests
- Poor edge case coverage
- Flaky or hard-to-read tests

## Design Patterns
- Introduce patterns only when they simplify or stabilize the design.
- Prefer composition over inheritance.
- Avoid unnecessary abstractions.

### Common patterns:

- **Strategy**: encapsulate algorithm variations
- **Factory**: manage object creation complexity
- **Adapter**: integrate external systems
- **Decorator**: extend behavior without modifying existing code

### When to act:
- Repeated conditional logic
- Poor separation of concerns
- Difficult extensibility

## References
Use these references when relevant to the detected context:

- For detailed rules, examples, and checklists, read `references/java-best-practices.md`.
- For Modern Java (17/21) features, read `references/modern-java.md`.
- For Build systems and Logging best practices, read `references/build-and-logs.md`.
- If the project is Spring Boot, read `references/spring-boot.md`.
- If the project is Quarkus, read `references/quarkus.md`.
- If the project is Struts, read `references/struts.md`.

