# Coding Standards

## Purpose

This document defines the coding standards for the Enterprise Commerce Platform.

The goal is to ensure consistency, readability, maintainability, and high code quality across all microservices.

---

# Java Version

- Java 21
- Use modern Java features where they improve readability.
- Prefer immutable objects where practical.

---

# Package Structure

Every microservice follows the same package structure.

```
com.ecp.<service>

├── config
├── controller
├── dto
├── entity
├── exception
├── mapper
├── repository
├── security
├── service
│   └── impl
├── validation
├── util
└── constant
```

Do not create arbitrary packages.

---

# Naming Conventions

## Classes

Use PascalCase.

Good

```
UserService

OrderController

PaymentRepository
```

Bad

```
userService

payment_controller
```

---

## Methods

Use camelCase.

```
createUser()

findOrderById()

calculateTotal()
```

---

## Variables

Use meaningful camelCase names.

Good

```
customerId

totalAmount

orderStatus
```

Avoid

```
a

temp

obj
```

---

## Constants

```
public static final
```

Example

```
MAX_LOGIN_ATTEMPTS

JWT_EXPIRATION

API_VERSION
```

---

# Controllers

Controllers should:

- Validate requests
- Delegate to services
- Return HTTP responses

Controllers must **not** contain business logic.

Example

```
Controller

↓

Service

↓

Repository
```

---

# Services

Business logic belongs only in the Service layer.

Services should:

- Be focused on one responsibility
- Be stateless
- Be transaction-aware

---

# Repositories

Repositories should only contain persistence logic.

Avoid business logic inside repositories.

---

# DTOs

Always expose DTOs.

Never expose JPA entities directly.

Example

```
Request DTO

↓

Service

↓

Entity

↓

Response DTO
```

---

# Entity Guidelines

Each entity should:

- Use `@Entity`
- Use `@Table`
- Use generated IDs
- Define relationships explicitly
- Avoid business logic

Entities should represent persistence only.

---

# Dependency Injection

Use constructor injection.

Good

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

Avoid field injection.

```java
@Autowired
private UserRepository repository;
```

---

# Logging

Use SLF4J.

Good

```java
log.info("User {} created successfully", userId);
```

Do not use

```
System.out.println()
```

Never log:

- Passwords
- JWTs
- Secrets
- Card numbers
- Personal data

---

# Exception Handling

Use a centralized `@RestControllerAdvice`.

Controllers should not contain `try-catch` blocks except where specifically required.

---

# Validation

Use Jakarta Bean Validation.

Example

```java
@NotBlank

@Email

@Positive

@Size
```

Validation belongs in DTOs.

---

# Transactions

Place `@Transactional` in the Service layer.

Avoid transactions in Controllers.

---

# Mapper

Use a dedicated mapper layer.

```
DTO

↓

Mapper

↓

Entity
```

Avoid manual mapping scattered across services.

---

# Configuration

Use `@ConfigurationProperties` for grouped configuration.

Avoid hardcoded values.

---

# REST APIs

Return meaningful HTTP status codes.

Use the standard `ApiResponse<T>` wrapper where applicable.

---

# Testing

Follow the Arrange–Act–Assert (AAA) pattern.

```
Arrange

↓

Act

↓

Assert
```

Naming examples:

```
shouldCreateUser()

shouldRejectInvalidPassword()

shouldReturn404WhenUserNotFound()
```

---

# Code Formatting

- UTF-8 encoding
- 4-space indentation
- One public class per file
- Remove unused imports
- Keep methods focused and short

---

# Documentation

Public APIs should be documented with OpenAPI annotations where appropriate.

Complex business logic should include concise comments explaining the reasoning, not the obvious implementation.

---

# Security Guidelines

- Never hardcode credentials
- Read secrets from environment variables or configuration
- Hash passwords with BCrypt
- Validate all external input
- Follow the principle of least privilege

---

# Performance Guidelines

- Avoid N+1 database queries
- Use pagination for large datasets
- Cache only when justified
- Use batch operations where appropriate
- Keep database transactions as short as possible

---

# Review Checklist

Before merging code:

- Builds successfully
- Tests pass
- No compiler warnings
- No duplicated code
- No unused dependencies
- Logging is appropriate
- Validation is present
- Exceptions are handled consistently
- API documentation updated (if applicable)