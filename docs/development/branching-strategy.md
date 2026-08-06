# Git Branching Strategy

## Purpose

This document defines the Git branching model used by the Enterprise Commerce Platform.

The objective is to support parallel development, stable releases, and predictable deployments while maintaining a clean Git history.

---

# Branching Model

```
main
│
└── develop
     │
     ├── feature/auth-service
     ├── feature/user-service
     ├── feature/product-service
     ├── feature/order-service
     ├── feature/payment-service
     ├── feature/inventory-service
     ├── feature/notification-service
     ├── feature/reporting-service
     ├── release/v1.0.0
     └── hotfix/fix-login-bug
```

---

# Branch Purpose

## main

Production-ready code.

Rules:

- Protected branch
- Direct commits are not allowed
- Only release merges
- Every merge is tagged

Example

```
v1.0.0

v1.1.0

v2.0.0
```

---

## develop

Integration branch.

Contains completed features that are ready for testing.

Rules

- Feature branches merge into develop
- Always buildable
- Automatically verified through CI

---

## feature/*

Used for individual features.

Examples

```
feature/auth-service

feature/user-service

feature/payment-service

feature/order-service

feature/kafka-integration
```

Rules

- Branch from develop
- Small focused changes
- Merge through Pull Request
- Delete after merge

---

## release/*

Created when preparing a production release.

Example

```
release/v1.0.0
```

Allowed changes

- Bug fixes
- Documentation
- Version updates
- Release notes

No new features.

---

## hotfix/*

Used to fix production issues.

Example

```
hotfix/jwt-expiration-fix
```

Rules

- Branch from main
- Merge into main
- Merge back into develop

---

# Development Workflow

```
develop

↓

feature/auth-service

↓

Commit

↓

Push

↓

Pull Request

↓

Code Review

↓

CI Build

↓

Merge into develop
```

---

# Pull Request Checklist

Before opening a Pull Request, ensure:

- Project builds successfully
- Unit tests pass
- No compilation warnings
- Code follows project standards
- Documentation updated if required
- API documentation updated
- No sensitive information committed

---

# Merge Strategy

Use:

```
--no-ff
```

Example

```bash
git checkout develop

git merge --no-ff feature/auth-service
```

Benefits

- Preserves feature history
- Easier release tracking
- Cleaner Git graph

---

# Commit Frequency

Prefer several small commits over one large commit.

Good

```
feat(auth): add login endpoint

feat(auth): implement JWT generation

test(auth): add login service tests
```

Avoid

```
Final changes

Updated code

Fixes
```

---

# Release Process

```
feature/*

↓

develop

↓

release/*

↓

main

↓

Tag

↓

Deploy
```

---

# Versioning

The project follows Semantic Versioning.

```
MAJOR.MINOR.PATCH
```

Examples

```
1.0.0

1.1.0

1.1.1

2.0.0
```

---

# Branch Protection Rules

Recommended repository settings:

- Require Pull Requests before merging
- Require successful CI build
- Require at least one review
- Prevent force pushes
- Prevent branch deletion for `main` and `develop`

---

# Git Tags

Each production release should be tagged.

Examples

```
v0.1.0-sprint0

v0.2.0-auth

v0.3.0-user

v1.0.0
```

---

# Best Practices

- Keep feature branches short-lived
- Rebase frequently with `develop`
- Resolve conflicts before creating a Pull Request
- Do not commit generated files or secrets
- Write meaningful commit messages
- Squash commits only when they improve readability

---

# Workflow Summary

```
Create Feature Branch

↓

Develop

↓

Commit Frequently

↓

Push

↓

Open Pull Request

↓

Code Review

↓

CI Verification

↓

Merge into develop

↓

Release Branch

↓

main

↓

Tag Release
```