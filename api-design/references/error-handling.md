# Standardized Error Handling

## Table of Contents
- RFC 7807 (Problem Details)
- Validation Errors
- Exception Handling Strategy
- Consistency in Responses
- Review Checklist

---

## RFC 7807 (Problem Details)

- **The Standard**: Use **Problem Details for HTTP APIs** (RFC 7807). This is now natively supported in **Spring Boot 3+** and **Quarkus**.
- **Core Fields**:
    - `type`: URI identifying the error type (use a documentation link).
    - `title`: Short, human-readable summary of the error.
    - `status`: The HTTP status code.
    - `detail`: Detailed explanation for this specific instance.
    - `instance`: URI identifying the specific occurrence of the error.

---

## Validation Errors

- **Structured Output**: For 400 Bad Request (Bean Validation failures), include a list of field-specific errors.
- **Format**:
  ```json
  {
    "type": "/errors/validation",
    "title": "Validation Failed",
    "status": 400,
    "errors": [
      { "field": "email", "message": "invalid format" },
      { "field": "age", "message": "must be at least 18" }
    ]
  }
  ```

---

## Exception Handling Strategy

- **Global Exception Handler**: Use `@ControllerAdvice` (Spring) or `ExceptionMapper` (Quarkus) to centralize error mapping.
- **Map Business Exceptions**: Create specific exceptions for business cases (e.g., `UserNotFoundException`) and map them to appropriate HTTP codes (404).
- **Avoid Leaking Internals**: Never include stack traces or internal database errors in the API response.

---

## Consistency in Responses

- **Same Format Everywhere**: Ensure that all endpoints return the same error structure.
- **Error Codes**: If the application uses internal error codes, include them as an additional field in the Problem Detail object.

---

## Review Checklist

- [ ] RFC 7807 format is respected.
- [ ] No stack traces are exposed in responses.
- [ ] Business exceptions are mapped to the correct HTTP status codes.
- [ ] Validation errors provide clear, field-specific feedback.
- [ ] A central exception handler is managing all error translations.
