# OpenAPI & Documentation Standards

## Table of Contents
- Controller Annotations
- DTO Annotations
- Documenting Errors
- Metadata & Summary
- Review Checklist

---

## Controller Annotations

- **Summary & Description**: Use `@Operation(summary = "...", description = "...")` for every endpoint.
- **Tags**: Group related endpoints using `@Tag(name = "...")` at the class level.
- **Response Codes**: Document all possible responses with `@ApiResponse`.
  ```java
  @ApiResponse(responseCode = "200", description = "Success")
  @ApiResponse(responseCode = "404", description = "User not found")
  ```

---

## DTO Annotations

- **Schema Description**: Use `@Schema(description = "...")` for classes and fields.
- **Required Fields**: Mark mandatory fields with `@Schema(requiredMode = RequiredMode.REQUIRED)`.
- **Examples**: Provide realistic example values with `@Schema(example = "...")`.
- **Validation Constraints**: Reflect `@Min`, `@Max`, `@Size` constraints in the OpenAPI schema.

---

## Documenting Errors

- **Problem Details**: Ensure error responses reference the `ProblemDetail` or equivalent schema.
- **Error Examples**: If possible, provide examples of validation error structures.

---

## Metadata & Summary

- **API Info**: Configure the global `Info` object (Title, Version, Description, Contact).
- **Servers**: List the different environments (Development, Staging, Production) in the documentation.

---

## Review Checklist

- [ ] Every endpoint has a summary and description.
- [ ] Tags are used to organize the UI.
- [ ] Success and error response codes (400, 404, etc.) are documented.
- [ ] DTO fields have clear descriptions and examples.
- [ ] Required fields are explicitly marked.
- [ ] The OpenAPI UI (Swagger UI) is clean and professional.
