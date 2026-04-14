# Modern Java Best Practices (Java 21+)

> **Note**: Java 21 is the minimum required version for this skill.

## Table of Contents
- Records for Data Carriers (Default)
- Sealed Classes & Exhaustive Switching
- Pattern Matching & Record Deconstruction
- Virtual Threads for Concurrency
- Review Checklist

---

## Records for Data Carriers (Default)

- **Standard**: Always use **Records** for DTOs, API responses, and internal data carriers.
- Use **Compact Constructors** for validation (e.g., `Objects.requireNonNull`).
- Leverage **Local Records** within methods to group intermediate data cleanly.

**When to act:**
- Replacing any class that is essentially a data wrapper (POJO).
- Avoiding Lombok `@Data` or `@Value` in favor of native Records.

---

## Sealed Classes & Exhaustive Switching

- Model domain states using `sealed` hierarchies (e.g., `sealed interface PaymentStatus permits Paid, Pending, Failed`).
- Use **Switch Expressions** to handle these hierarchies; the compiler will enforce exhaustiveness, removing the need for a `default` clause that hides bugs.

---

## Pattern Matching & Record Deconstruction

- Use **Record Patterns** to destructure data directly:
  ```java
  if (obj instanceof User(String name, int age)) {
      System.out.println(name + " is " + age);
  }
  ```
- Use **Unnamed Variables (`_`)** for components you don't need during deconstruction (Java 21 feature).

---

## Virtual Threads (Project Loom)

- **Default for I/O**: Use Virtual Threads for all blocking I/O operations.
- **Simplification**: Replace complex CompletableFuture chains or Reactive code with simple, synchronous-style code running on virtual threads.
- Use `Executors.newVirtualThreadPerTaskExecutor()`.
- **Constraint**: Avoid `synchronized` for long I/O; use `ReentrantLock` to prevent thread pinning.

---

## Review Checklist

- [ ] All data carriers are implemented as `record`.
- [ ] Sealed hierarchies used for domain states with exhaustive `switch`.
- [ ] Switch expressions used instead of `if-else` blocks.
- [ ] Virtual threads utilized for I/O-bound tasks.
- [ ] No manual casting (use Pattern Matching).
- [ ] Java 21 specific features (unnamed variables, record patterns) are utilized where appropriate.
