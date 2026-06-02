---
name: auth
description: Designs and implements authentication and authorization systems — JWT, OAuth, sessions, API keys, RBAC. Use when adding login, auth middleware, permissions, or security policies.
---

# Authentication & Authorization

Designs and implements authentication and authorization systems following security best practices.

## How it works

1. Identify the auth requirements: type (JWT, session, OAuth, API key), user roles, token lifecycle
2. Detect existing auth patterns in the project
3. Design the auth flow: registration, login, token refresh, logout, password reset
4. Implement auth middleware/guards with proper error handling
5. Add authorization checks (RBAC, ABAC, or custom)
6. Secure sensitive endpoints and data

## Auth types

### JWT (JSON Web Tokens)
- Access tokens (short-lived) + refresh tokens (long-lived)
- Store refresh tokens securely (httpOnly cookies or DB)
- Never store JWTs in localStorage for web apps
- Include minimal claims (sub, role, exp)

### Session-based
- Server-side session store (Redis, database)
- httpOnly, secure, sameSite cookies
- Session rotation on privilege changes

### OAuth / OIDC
- Use established providers (Google, GitHub, Auth0, etc.)
- Implement PKCE for public clients
- Store tokens securely, never in URLs
- Handle token refresh automatically

### API Keys
- Generate cryptographically secure keys
- Hash before storing (like passwords)
- Support key rotation and revocation
- Rate limit per key

## Authorization patterns

### RBAC (Role-Based Access Control)
- Define roles (admin, user, moderator, etc.)
- Map permissions to roles
- Check permissions at route/handler level

### ABAC (Attribute-Based Access Control)
- Policies based on user attributes, resource attributes, and context
- More flexible than RBAC for complex systems

## Security checklist

- [ ] Passwords hashed with bcrypt/argon2 (not MD5/SHA1)
- [ ] Rate limiting on auth endpoints
- [ ] Account lockout after failed attempts
- [ ] Secure password requirements enforced
- [ ] Tokens have appropriate expiration
- [ ] Refresh token rotation implemented
- [ ] CORS configured correctly
- [ ] Auth middleware on all protected routes
- [ ] No secrets in client-side code
- [ ] HTTPS enforced for auth endpoints
- [ ] Session fixation protection
- [ ] CSRF protection for session-based auth

## Output format

```markdown
## Authentication System

**Type:** JWT / Session / OAuth / API Key
**Storage:** httpOnly cookies / Redis / Database

### Endpoints
| Method | Path | Description |
|--------|------|-------------|
| POST | /auth/register | Create account |
| POST | /auth/login | Authenticate |
| POST | /auth/refresh | Refresh token |
| POST | /auth/logout | Invalidate session |

### Middleware
- `authMiddleware` — validates token/session
- `roleMiddleware(roles)` — checks user role
- `permissionMiddleware(perms)` — checks permissions

### Files created
- `middleware/auth.ts` — Authentication middleware
- `controllers/auth.ts` — Auth endpoints
- `types/auth.ts` — Auth type definitions
```
