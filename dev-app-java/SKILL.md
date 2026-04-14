---
name: dev-app-java
description: "End-to-end Java application development (creation, structuring, refactoring, quality review) for Vanilla Java, Spring Boot, Quarkus, or Struts. Use it whenever you need to write Java applications, generate projects, apply best practices (Javadoc, unit tests, design patterns), or guide architecture and code quality."
---

# Dev App Java

## Overview

Apply cross-cutting Java best practices (Javadoc, unit tests, design patterns) on existing or new code, with **Java 21** as the baseline version.

## Workflow

1. **Clarify Context**
   - Identify the application type (Vanilla Java, Spring Boot, Quarkus, Struts) and the immediate goal.
   - If a framework is mentioned, respect its conventions without assuming specific APIs.

2. **Diagnose Needs**
   - Javadoc: missing contracts, ambiguities, exceptions, or examples.
   - Tests: case coverage, isolation, weak assertions, brittle tests.
   - Design patterns: duplicated logic, mixed responsibilities, need for extensibility.

3. **Apply Targeted Best Practices**
   - Use the summary rules below and the detailed reference.
   - Propose minimal, safe, and justified changes.

4. **Validate**
   - Check readability, consistency, and impact on the rest of the code.
   - Propose associated unit tests if behavior changes.

## Javadoc

- Write a clear contract: role, preconditions, postconditions, invariants.
- Document parameters, return values, exceptions (including conditions).
- Avoid redundancy with the method name: explain the "why".
- Add an example if usage is not obvious.
- Update Javadoc whenever signature or behavior changes.

**When to act:** public method without an explicit contract, implicit behaviors, poorly documented possible errors.

## Unit Testing

- Use JUnit 5 by default; isolate units (mocks/stubs) if necessary.
- Cover nominal, edge, error, and invariant cases.
- Avoid brittle tests (time dependencies, order, global data).
- Prefer precise and readable assertions; name tests by behavior.
- Minimize duplication via helpers/factories if useful.

**When to act:** lack of tests, low coverage of edge cases, flaky tests.

## Design Patterns

- Introduce a pattern only if it simplifies or stabilizes the design.
- Prefer composition over inheritance.
- Common patterns:
  - **Strategy**: algorithm variants or rules.
  - **Factory**: complex construction or multiple implementations.
  - **Adapter**: integration of external APIs.
  - **Decorator**: additional responsibilities without class explosion.
- Justify the chosen pattern with a concrete problem.

**When to act:** repeated conditional logic, "god object" class, difficult extension.

## References

- For detailed rules, examples, and checklists, read `references/java-best-practices.md`.
- For Modern Java (17/21) features, read `references/modern-java.md`.
- For Build systems and Logging best practices, read `references/build-and-logs.md`.
- If the project is Spring Boot, read `references/spring-boot.md`.
- If the project is Quarkus, read `references/quarkus.md`.
- If the project is Struts, read `references/struts.md`.

