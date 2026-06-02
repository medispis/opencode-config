---
name: git-workflow
description: Manages advanced git workflows — branching strategies, PR automation, changelog generation, and team collaboration. Use when setting up branch protection, automating PRs, managing releases, or standardizing team git practices.
---

# Git Workflow

Manages advanced git workflows, branching strategies, PR automation, and team collaboration patterns.

## How it works

1. Detect the current git workflow (GitHub Flow, Git Flow, trunk-based)
2. Identify gaps in the current setup
3. Generate configuration for branch protection, PR templates, commit conventions
4. Set up automation for changelogs, versioning, and release workflows

## Branching strategies

### GitHub Flow (recommended for most teams)
- `main` is always deployable
- Feature branches: `feat/description`
- PRs for every change
- Deploy after merge

### Git Flow (for release-heavy projects)
- `main` for production
- `develop` for integration
- `feature/*`, `release/*`, `hotfix/*`
- More complex, better for scheduled releases

### Trunk-Based (for mature CI/CD)
- Short-lived feature branches (<1 day)
- Feature flags for incomplete work
- Continuous deployment

## PR automation

### PR template
Generate `.github/PULL_REQUEST_TEMPLATE.md`:
```markdown
## Summary
[What changed and why]

## Testing
- [ ] Unit tests added/updated
- [ ] Manual testing done
- [ ] No breaking changes

## Checklist
- [ ] Code follows project conventions
- [ ] Self-reviewed
- [ ] Documentation updated
```

### Conventional commits
Enforce commit message format:
```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `ci`, `style`

## Automation setup

### GitHub Actions for PR checks
- Lint and typecheck on every PR
- Run test suite
- Check commit message format
- Auto-label PRs based on changed files

### Changelog automation
- Generate from conventional commits
- Group by type (features, fixes, breaking)
- Include links to commits/PRs

## Rules

- Detect existing workflow before generating — don't overwrite
- Keep branching strategy simple unless the team needs complexity
- PR templates should be short and actionable
- Don't add automation that slows down the developer loop
- Support GitHub, GitLab, and Bitbucket

## Output format

```markdown
## Git Workflow Setup

**Strategy:** GitHub Flow / Git Flow / Trunk-Based

### Configuration
- `.github/PULL_REQUEST_TEMPLATE.md` — PR template
- `.github/commitlint.config.js` — Commit message format
- `.github/workflows/pr-checks.yml` — PR automation

### Branch protection
- Require PR reviews (min 1)
- Require status checks to pass
- Require branches to be up to date
- Dismiss stale reviews on new commits

### Automation
- PR checks: lint, test, typecheck
- Auto-label: based on changed files
- Changelog: generated from conventional commits
```
