---
name: backend-development
description: >-
  Backend engineering procedures for API scaffolding (REST/GraphQL), database query optimization, service integration, and business logic implementation with clean architecture.
---

# Backend Development Skill

Provides engineering standards, code structure guidelines, and token-optimized implementation workflows for Senior Backend Developers.

---

## ⚡ Token Optimization Note (Implementation-Only Mode)
By default, backend engineers operate in **Implementation-Only Mode**:
- Do not write, execute, or read unit test files unless explicitly requested by the user (`"write test"`, `"unit test"`, etc.).
- Rely on schema validation models (Pydantic, Zod) and static type checking (`mypy --quick`, `tsc --noEmit`) for deterministic baseline correctness.

---

## 1. Core Workflows & Procedures

### Workflow 1: API Scaffolding & Routing
1. Read API contracts from `docs/ARCHITECTURE.md` or OpenAPI specifications.
2. Implement route handlers, request validation models (e.g. Pydantic / Zod), and response serialization.
3. Enforce standardized HTTP status codes (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `422 Unprocessable Entity`, `500 Internal Error`).
4. Ensure global exception handling returns uniform JSON error envelopes:
   ```json
   {
     "error": {
       "code": "RESOURCE_NOT_FOUND",
       "message": "User with ID '123' was not found",
       "details": {}
     }
   }
   ```

### Workflow 2: Business Logic & Clean Architecture Layering
- **Controller/Router Layer**: Parses requests, validates headers/params, calls services.
- **Service/Domain Layer**: Houses pure business logic, orchestration, and domain rules.
- **Repository/Data Layer**: Encapsulates database queries (ORM / SQL), caching, and persistence.
- Never leak raw database queries into HTTP controllers.

### Workflow 3: Database Query Optimization & Transactions
- Avoid N+1 query problems by using eager loading / joins (`select_related`, `prefetch_related`, `JOIN FETCH`).
- Wrap multi-table mutation operations in atomic transactions with rollback safeguards.
- Add database indexes on frequently filtered/joined foreign keys.

### Workflow 4: Third-Party Service Integration
- Implement resilient HTTP client wrappers with timeout caps, retries, and circuit breakers.
- Store all secrets and URLs in environment variables (never hardcode in source).

---

## 2. Backend Verification Checklist

Before completing backend implementation tasks, verify:
- [ ] All request payloads are strictly validated against schema models.
- [ ] Linter and type-checker pass with 0 errors (`mypy --quick`, `ruff check`, or `tsc --noEmit`).
- [ ] Strict Zero-Test Policy observed: unit tests written or run ONLY if explicitly requested by user prompt.
- [ ] No hardcoded credentials, secret keys, or absolute local paths.
