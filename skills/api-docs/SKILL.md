---
name: api-docs
description: Generates API documentation from existing code — OpenAPI specs, README sections, SDK docs. Use when documenting endpoints, generating specs, or syncing docs after API changes.
---

# API Documentation

Generates accurate API documentation by reading actual code — never guessing or templating.

## Quick start

1. Scan codebase for route definitions, handlers, controllers
2. Extract HTTP methods, paths, parameters, request bodies, response shapes
3. Read type definitions, DTOs, schemas for structure
4. Infer authentication from middleware usage
5. Generate documentation in requested format

## Detection

Scan for API patterns:
- **Express/Fastify**: `router.get()`, `app.post()`, route files
- **FastAPI**: `@app.get()`, `@router.post()`, Pydantic models
- **Gin**: `r.GET()`, `r.POST()`, handler functions
- **Spring**: `@GetMapping`, `@PostMapping`, controller classes
- **NestJS**: `@Get()`, `@Post()`, controller decorators
- **GraphQL**: `type Query`, `type Mutation`, resolver files

## Output formats

### OpenAPI spec
Generate `openapi.yaml` with info block, paths, component schemas, security schemes, and examples.

### README section
Generate markdown with endpoint summary table, detailed docs per endpoint, authentication section, error response format, and example requests (curl + SDK).

### Both
Generate OpenAPI spec + a README section that links to it.

## Rules

- Read actual code to generate docs — don't guess or template
- Keep docs accurate to what the code actually does, not what it should do
- If response types can't be inferred, mark as `unknown` rather than fabricating
- If OpenAPI spec already exists, update it rather than overwriting
- Support all frameworks and languages

## Output format

```markdown
## API Documentation

### Endpoints
| Method | Path | Description | Auth |
|--------|------|-------------|------|
| GET | /api/users | List all users | JWT |
| POST | /api/users | Create a user | JWT |

### OpenAPI spec
Generated: `openapi.yaml` (v3.0.3)

### Files created/modified
- `openapi.yaml` - Full OpenAPI specification
- `README.md` - Added API reference section
```
