---
name: api-scaffold
description: Generates API endpoints, routes, and handlers from specifications or existing patterns. Use when adding new API endpoints, scaffolding CRUD operations, or generating code from OpenAPI specs.
---

# API Scaffolding

Generates REST or GraphQL API endpoints following existing project patterns for middleware, auth, and response formatting.

## How it works

1. Analyze existing API patterns in the project (routes, controllers, middleware)
2. Identify the framework and conventions
3. Generate route definitions with proper HTTP methods and paths
4. Create handler/controller functions with input validation, error handling, auth checks, and response formatting
5. Generate or update type definitions (interfaces, DTOs, schemas)
6. Add or update API documentation

## Framework patterns

Detect and follow the project's framework:
- **Express/Fastify**: Router files, middleware chains, controller pattern
- **FastAPI/Flask**: Router modules, Pydantic models, dependency injection
- **Gin/Echo**: Handler functions, middleware, struct binding
- **Spring Boot**: Controller classes, service layer, DTO pattern
- **NestJS**: Modules, controllers, services, guards, pipes
- **GraphQL**: Resolvers, type definitions, schema-first or code-first

## Conflict handling

- **Same endpoint, different file**: Create alongside, don't overwrite
- **Same file, append**: Add new routes to existing route files
- **Overwrite needed**: Show diff and ask for confirmation before overwriting

## Versioned APIs

If the project uses versioned routes (e.g., `/api/v1/...`):
- Follow the existing version pattern
- Don't create a new version unless asked
- Place new endpoints in the current version by default

## Output format

```markdown
### [METHOD] /path

**Description:** What this endpoint does

**Request:**
- Params: `id: string`
- Body: `{ field: type }`
- Auth: [none/jwt/api-key/role-based]

**Response:**
- 200: `{ data: ... }`
- 400: `{ error: "validation message" }`
- 401: `{ error: "unauthorized" }`
- 404: `{ error: "not found" }`

**Files created/modified:**
- `routes/resource.ts` - Route definition
- `controllers/resource.ts` - Handler logic
- `types/resource.ts` - Type definitions
```

## After scaffolding

Invoke @test to write tests for the new endpoints.
