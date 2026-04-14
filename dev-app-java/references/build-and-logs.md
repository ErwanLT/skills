# Build Systems & Structured Logging

## Table of Contents
- Maven Best Practices
- Gradle Best Practices
- Quality & Lifecycle Plugins
- Structured Logging
- Review Checklist

---

## Maven Best Practices

- **BOM (Bill of Materials)**: Use BOMs (e.g., Spring Boot BOM, Quarkus BOM) to manage dependency versions consistently across modules.
- **Multi-Module Projects**: Structure large apps into modules (e.g., `api`, `core`, `infrastructure`) to enforce boundaries.
- **Dependency Management**: Centralize versions in the `<dependencyManagement>` section of the root POM.
- **Clean POMs**: Keep the main `<dependencies>` list minimal; inherit from parent or management.

**When to act:**
- Version mismatches between dependencies.
- Monolithic project structure becoming hard to maintain.

---

## Gradle Best Practices

- **Java Version**: Set `java.toolchain.languageVersion = JavaLanguageVersion.of(21)`.
- **Version Catalog**: Use `libs.versions.toml` to centralize all dependency versions and plugins.
- **Project Accessors**: Use type-safe project accessors in multi-project builds.
- **Convention Plugins**: Use `buildSrc` or `composite builds` to share common build logic (test config, checkstyle) instead of huge `allprojects` blocks.

---

## Quality & Lifecycle Plugins

- **JaCoCo**: Enforce minimum code coverage (e.g., 80%) in the build pipeline.
- **Spotless / Checkstyle**: Automate code formatting and style checks.
- **Maven Enforcer Plugin**: Ensure specific Java versions and prohibit "banned" dependencies.

---

## Structured Logging

- **JSON Format**: Use structured logging (JSON) in production for easier indexing in ELK/Splunk.
- **Contextual Data (MDC)**: Use `MDC` (Mapped Diagnostic Context) to attach `requestId`, `userId`, or `correlationId` to every log line.
- **Log Levels**:
    - `ERROR`: Action required.
    - `WARN`: Unusual but system continues.
    - `INFO`: Key lifecycle events (startup, business milestones).
    - `DEBUG`: Troubleshooting details (never in production).
- **Parametrized Messages**: Always use `{}` placeholders instead of string concatenation (e.g., `logger.info("User {} logged in", userId)`).

**When to act:**
- Hard-to-parse logs in production.
- Missing correlation IDs between service calls.
- String concatenation in log statements.

---

## Review Checklist

- [ ] Dependencies managed via BOM or Version Catalog.
- [ ] No hardcoded versions in sub-modules.
- [ ] Quality plugins (JaCoCo, Checkstyle) configured.
- [ ] Log messages use placeholders `{}`.
- [ ] MDC used for tracing requests.
- [ ] JSON logging enabled for production profiles.
used for tracing requests.
- [ ] JSON logging enabled for production profiles.
