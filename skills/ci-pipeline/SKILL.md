---
name: ci-pipeline
description: Generates CI/CD pipeline configurations for GitHub Actions, GitLab CI, CircleCI, and more. Use when setting up CI/CD, adding workflows, migrating platforms, or optimizing pipelines.
---

# CI/CD Pipeline

Generates CI/CD pipeline configurations from project stack with lint, test, build, and deploy stages.

## How it works

1. Detect the project's language, framework, package manager
2. Identify existing CI config (if any)
3. Detect test, build, and lint commands from package.json/Makefile
4. Generate pipeline config with triggers, job definitions, caching, matrix builds, and secret references
5. Generate deployment jobs if requested

## Platform detection

Check for existing CI files:
- `.github/workflows/` → GitHub Actions
- `.gitlab-ci.yml` → GitLab CI
- `.circleci/config.yml` → CircleCI
- `Jenkinsfile` → Jenkins
- `bitbucket-pipelines.yml` → Bitbucket Pipelines

If none exist, ask the user which platform. Default to GitHub Actions.

## Pipeline stages

### Standard pipeline
```
lint → test → build → deploy
```

### With matrix
```
lint → test [matrix: node 18, 20, 22] → build → deploy
```

### PR pipeline (lighter)
```
lint → test
```

## Best practices enforced

- Cache dependencies between runs
- Use matrix builds for compatibility testing
- Separate CI (test) from CD (deploy) workflows
- Use environment protection rules for production deploys
- Pin action versions (e.g., `actions/checkout@v4`)
- Use secrets, never hardcode credentials
- Add status badges to README
- Set appropriate timeouts to prevent hung jobs

## Rules

- Detect existing CI config before generating — don't overwrite without asking
- Follow the project's existing conventions if CI already exists
- Generate minimal, focused workflows — don't add stages the user didn't ask for
- If Dockerfile exists, include docker build/push in the pipeline

## Output format

```markdown
## CI/CD Pipeline

**Platform:** GitHub Actions
**Trigger:** push to main, pull requests

### Workflows generated

#### ci.yml
- **Triggers:** push, pull_request
- **Jobs:** lint, test, build
- **Matrix:** Node.js 18, 20, 22
- **Caching:** npm, node_modules

### Secrets needed
| Secret | Purpose |
|--------|---------|
| DEPLOY_KEY | SSH key for deployment |
| REGISTRY_TOKEN | Container registry auth |
```
