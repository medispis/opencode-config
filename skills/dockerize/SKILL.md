---
name: dockerize
description: Creates Dockerfiles, docker-compose configs, and containerization setups. Use when containerizing apps, adding services to docker-compose, or optimizing Docker builds.
---

# Dockerize

Generates optimized Dockerfiles, docker-compose configs, multi-stage builds, and .dockerignore files.

## How it works

1. Detect the project's language, framework, and dependencies
2. Identify entry points, build commands, and runtime requirements
3. Generate a Dockerfile with appropriate base image, multi-stage build, proper layer caching, non-root user, and health check
4. Generate docker-compose.yml with service definitions, volume mounts, environment variables, and dependency ordering
5. Generate .dockerignore to exclude unnecessary files

## Secret management

**NEVER bake secrets into Docker images.**
- Use environment variables for runtime config
- Reference `.env` files via `env_file` in docker-compose
- Add `.env` to `.dockerignore` (never COPY it into the image)
- Document which env vars are required in `.env.example`

## Best practices enforced

- Use specific image tags (not `latest`)
- Minimize image layers
- Run as non-root user
- Use .dockerignore
- Set HEALTHCHECK
- Use multi-stage builds for compiled languages
- Pin dependency versions
- Use COPY instead of ADD
- Order instructions from least to most frequently changing

## Output format

```markdown
## Docker Configuration

### Dockerfile
- **Base image:** node:20-alpine
- **Stages:** build, production
- **Port:** 3000
- **User:** node (non-root)

### docker-compose.yml
- **Services:** app, db, redis
- **Networks:** app-network
- **Volumes:** db-data, redis-data
- **Env file:** .env (referenced, not copied)

### .dockerignore
- Excludes: node_modules, .git, *.md, .env, .env.*
```
