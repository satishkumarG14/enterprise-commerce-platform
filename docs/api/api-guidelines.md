# API Design Guidelines

## Purpose

This document defines the REST API standards followed across all microservices in the Enterprise Commerce Platform.

The objective is to ensure consistency, maintainability, backward compatibility, and a predictable developer experience.

---

# General Principles

- RESTful APIs
- Stateless services
- JSON request/response bodies
- HTTPS only
- Versioned APIs
- Resource-oriented URLs
- Consistent error responses
- Idempotent operations where applicable

---

# API Versioning

All APIs must be versioned.

Example:

```
/api/v1/auth/login
/api/v1/users
/api/v1/orders
```

Major version changes introduce breaking changes.

---

# URI Naming

## Use nouns

Good

```
GET /users
GET /products
GET /orders
```

Bad

```
GET /getUsers
GET /createProduct
POST /saveOrder
```

---

## Collections

```
GET /users
GET /products
GET /orders
```

---

## Single Resource

```
GET /users/{id}
GET /products/{id}
GET /orders/{id}
```

---

## Nested Resources

```
GET /users/{id}/orders

GET /orders/{id}/items
```

---

# HTTP Methods

| Method | Usage |
|---------|------|
| GET | Retrieve resource |
| POST | Create resource |
| PUT | Replace resource |
| PATCH | Partial update |
| DELETE | Delete resource |

---

# HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Resource Created |
| 202 | Accepted |
| 204 | No Content |
| 400 | Validation Error |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Business Validation Failure |
| 500 | Internal Server Error |

---

# Standard Success Response

```json
{
  "success": true,
  "message": "User created successfully",
  "data": {
      ...
  }
}
```

---

# Standard Error Response

```json
{
  "timestamp": "2026-08-06T10:30:00Z",
  "status": 400,
  "error": "Validation Failed",
  "message": "Email is required",
  "path": "/api/v1/users"
}
```

---

# Pagination

Use page-based pagination.

Example

```
GET /products?page=0&size=20&sort=name,asc
```

Response

```json
{
    "content": [],
    "page": 0,
    "size": 20,
    "totalElements": 250,
    "totalPages": 13
}
```

---

# Filtering

Use query parameters.

```
GET /products?category=electronics

GET /orders?status=PAID

GET /users?active=true
```

---

# Sorting

```
GET /products?sort=name,asc

GET /orders?sort=createdAt,desc
```

---

# Date Format

Use ISO-8601.

Example

```
2026-08-06T14:30:45Z
```

---

# Validation

Bean Validation must be used.

Example

```java
@NotBlank
@Email
@Size(max = 100)
```

Validation failures return HTTP 400.

---

# Authentication

Protected APIs require:

```
Authorization: Bearer <JWT>
```

---

# Idempotency

The following operations must be idempotent.

```
PUT

DELETE
```

For payment-related POST requests, support an `Idempotency-Key` header to safely retry requests without creating duplicate transactions.

---

# Correlation ID

Every incoming request must contain or generate:

```
X-Correlation-ID
```

The value should be propagated to downstream services and included in application logs.

---

# Logging

Never log:

- Passwords
- JWT Tokens
- Credit Card Numbers
- CVV
- Personal Secrets

---

# OpenAPI

Every service must expose:

```
/v3/api-docs

/swagger-ui.html
```

using SpringDoc OpenAPI.

---

# Backward Compatibility

Breaking API changes require:

- New API version
- Migration documentation
- Deprecation notice

---

# Naming Conventions

Use:

```
camelCase

JSON

English

Meaningful field names
```

Example

```json
{
    "firstName": "John",
    "lastName": "Doe",
    "createdAt": "2026-08-06T12:00:00Z"
}
```

---

# API Design Checklist

Before publishing an endpoint, verify:

- Resource-oriented URI
- Correct HTTP method
- Proper status code
- Bean Validation
- JWT protection (if required)
- Standard response structure
- Pagination support (if applicable)
- OpenAPI documentation
- Correlation ID logging
- Unit and integration tests