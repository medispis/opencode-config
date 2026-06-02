---
name: dev-env
description: Sets up development environment configurations, tooling, monorepo structure, and project scaffolding. Use when initializing projects, adding linting/formatting, configuring git hooks, or setting up monorepos.
---

# Dev Environment

Sets up development environment configurations, tooling, monorepo structure, and project scaffolding.

## Tooling setup

1. Detect the project's language, framework, and package manager
2. Identify existing configuration and gaps
3. Generate configuration files for linting, formatting, type checking, git hooks, and editor settings
4. Create `.env.example` with documented variables
5. Set up package.json scripts or Makefile targets

### Tool installation

After generating config files, run the appropriate install commands:

| Ecosystem | Install command |
|-----------|----------------|
| Node.js (npm) | `npm install -D eslint prettier husky lint-staged` |
| Node.js (pnpm) | `pnpm add -D eslint prettier husky lint-staged` |
| Python | `pip install ruff black pre-commit mypy` |
| Go | `go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest` |
| Rust | `rustup component add clippy rustfmt` |

### Environment variable validation

When generating `.env.example`:
- Scan the codebase for `process.env.X`, `os.environ["X"]`, `env("X")` patterns
- Document every variable found
- Mark required vs optional based on usage
- Include realistic examples, not placeholders

## Monorepo setup

For monorepo projects (detected via workspaces, lerna.json, nx.json, turbo.json):

### Tool selection

| Tool | Best for | Complexity |
|------|----------|------------|
| Turborepo | Most projects, simple setup | Low |
| Nx | Large teams, many packages, plugins | Medium-High |
| Lerna | Package publishing to npm | Low |
| pnpm workspaces | pnpm users, simple needs | Low |

### Workspace structure

```
monorepo/
├── apps/              # Deployable applications
├── packages/          # Shared libraries
├── tools/             # Build tools, scripts
├── turbo.json         # Task pipeline config
├── tsconfig.base.json # Shared TypeScript config
└── package.json       # Root package.json
```

### Shared configurations

- **TypeScript**: `tsconfig.base.json` at root, each package extends base
- **ESLint**: Shared config in `packages/config/eslint/`
- **Prettier**: Single `.prettierrc` at root

### Task pipelines

```json
{
  "pipeline": {
    "build": { "dependsOn": ["^build"], "outputs": ["dist/**"] },
    "test": { "dependsOn": ["build"] },
    "lint": {},
    "dev": { "cache": false, "persistent": true }
  }
}
```

## Rules

- Detect existing setup before generating — don't overwrite without asking
- If the project already uses workspaces, enhance rather than replace
- Keep the setup minimal — don't add packages the user didn't ask for
- Configure caching by default
- Set up proper TypeScript project references for IDE support
- Support all major monorepo tools

## Output format

```markdown
## Environment Setup

### Tools configured
| Tool | Config file | Purpose | Installed |
|------|-------------|---------|-----------|
| ESLint | `.eslintrc.js` | Code linting | yes |
| Prettier | `.prettierrc` | Code formatting | yes |

### Environment variables
| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| DATABASE_URL | Yes | PostgreSQL connection string | postgres://localhost:5432/mydb |

### Files created
- `.eslintrc.js` - Linting rules
- `.env.example` - Environment template
- `turbo.json` - Task pipeline config
```
