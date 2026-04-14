# RESTful API Guidelines

## Table of Contents
- Resource Naming & URL Structure
- HTTP Methods & Semantics
- HTTP Status Codes
- Pagination & Filtering
- Versioning
- Review Checklist

---

## Resource Naming & URL Structure

- **Nouns**: Use plural nouns for resources (e.g., `/orders`, not `/getOrders`).
- **Hierarchy**: Reflect relationships in the path (e.g., `/users/{id}/orders/{orderId}`).
- **Case**: Prefer `kebab-case` for path segments (e.g., `/user-profiles`).
- **Filtering**: Use query parameters for filtering, sorting, and pagination (e.g., `/orders?status=SHIPPED&page=1`).

---

## HTTP Methods & Semantics

- **GET**: Retrieve a resource or a list. **Safe** (no side effects) and **Idempotent**.
- **POST**: Create a new resource or perform a non-idempotent action.
- **PUT**: Replace an existing resource or create if not exists (if ID is provided by client). **Idempotent**.
- **PATCH**: Partial update of an existing resource.
- **DELETE**: Remove a resource. **Idempotent**.

---

## HTTP Status Codes

- **2xx (Success)**
    - `200 OK`: Successful retrieval or update.
    - `201 Created`: Successful creation (include `Location` header).
    - `204 No Content`: Successful execution with no response body (e.g., DELETE).
- **4xx (Client Errors)**
    - `400 Bad Request`: Validation failure or malformed request.
    - `401 Unauthorized`: Authentication required (not handled by this skill, but keep in mind).
    - `403 Forbidden`: Authenticated but not allowed (not handled by this skill).
    - `404 Not Found`: Resource does not exist.
    - `405 Method Not Allowed`: HTTP method not supported on this URL.
- **5xx (Server Errors)**
    - `500 Internal Server Error`: Unexpected error on the server.
    - `503 Service Unavailable`: Temporary overload or maintenance.

---

## Pagination & Filtering

- **Standardization**: Use consistent parameters for pagination (e.g., `page` and `size`).
- **Response**: Return pagination metadata (total elements, total pages, current page) in the response body or headers.
- **Sorting**: Support a `sort` parameter (e.g., `sort=createdAt,desc`).

---

## Versioning

- **Header Versioning**: Prefer versioning via headers (e.g., `Accept: application/vnd.company.v1+json`).
- **URL Versioning**: Alternatively, use `/v1/` prefix in the URL if required by the project.
- **Avoid breaking changes**: Always maintain backward compatibility or provide a clear migration path.

---

## Review Checklist

- [ ] URLs use plural nouns and kebab-case.
- [ ] Proper HTTP methods are used according to semantics.
- [ ] Accurate HTTP status codes are returned.
- [ ] Response DTOs are used (no entities exposed).
- [ ] Input validation is implemented at the Controller level.
- [ ] GET methods are safe and idempotent.
