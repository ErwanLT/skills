---
name: api-design
description: "Design and implementation of robust, clear, and documented RESTful APIs. Focus on the 'Controller' part of the Controller/Service/Repository pattern, ensuring consistent responses, proper HTTP usage, and comprehensive OpenAPI documentation."
---

# API Design & Documentation

## Overview

Ensure the API is intuitive, well-documented, and adheres to RESTful standards. This skill focuses on the entry points of your application (Controllers), mapping business results to HTTP responses, and providing clear contracts for consumers.

## Workflow

1. **Design the Resource Model**
   - Identify resources and their relationships.
   - Define clear, noun-based URL structures (e.g., `/users/{id}/orders`).
   - Choose appropriate HTTP methods (GET, POST, PUT, PATCH, DELETE).

2. **Define Data Contracts (DTOs)**
   - Create dedicated DTOs for requests and responses.
   - **Never expose internal entities** (JPA entities or internal records) directly to the API.
   - Ensure DTOs are immutable (using Java 21 Records).

3. **Standardize Responses & Errors**
   - Use correct HTTP status codes (200, 201, 204, 400, 404, 500).
   - Implement standardized error responses using **Problem Details (RFC 7807)**.
   - Ensure consistency across all endpoints.

4. **Document the API**
   - Annotate Controllers and DTOs with **OpenAPI/Swagger** annotations.
   - Provide clear descriptions, example values, and documented error cases.

## Core Principles

### RESTful Standards
- Use **Nouns** for URLs, not verbs.
- **GET** is safe and idempotent (no side effects).
- **PUT** replaces a resource (idempotent).
- **PATCH** partially updates a resource.
- **POST** creates a resource (not idempotent).

### Controller Layer Responsibilities
- Validate incoming data (using `@Valid` or manual checks).
- Map incoming DTOs to Service-level objects.
- Call the appropriate Service methods.
- Map Service results or exceptions to HTTP responses/DTOs.
- **Keep it thin**: No business logic should reside in the Controller.

### Documentation Quality
- Every endpoint must have a summary and a description.
- Document all possible response codes.
- Use examples to help API consumers understand the data format.

## References

- For REST guidelines and HTTP usage, read `references/rest-guidelines.md`.
- For OpenAPI and documentation standards, read `references/openapi-docs.md`.
- For standardized error handling and validation, read `references/error-handling.md`.
