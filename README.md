# OpenCode Agent Configuration

Personal configuration for OpenCode AI — a multi-agent system with specialized agents and skills for software development workflows.

[![GitHub](https://img.shields.io/badge/GitHub-medispis/opencode--config-181717?style=flat-square&logo=github)](https://github.com/medispis/opencode-config)

---

## Table of Contents

- [Agents](#agents)
- [Skills](#skills)
- [Getting Started](#getting-started)
- [Requirements](#requirements)
- [Quick Install](#quick-install)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

---

## Agents

This configuration defines **17 specialized agents**, each tuned for a specific domain in the software development lifecycle. Agents are invoked via `@name` and have dedicated temperature settings optimized for their role.

| Agent | Purpose |
|-------|---------|
| `@orchestrator` | Breaks down complex tasks and delegates to specialists |
| `@build` | Implements features, writes code, and makes changes to the codebase |
| `@debug` | Investigates bugs, errors, and unexpected behavior using the RIVAH framework |
| `@test` | Writes, updates, and runs tests for code verification |
| `@review` | Reviews code for quality, bugs, security issues, and best practices |
| `@security` | Performs security audits and identifies vulnerabilities in code and dependencies |
| `@docs` | Creates and maintains project documentation, READMEs, API docs, and code comments |
| `@plan` | Designs technical architecture and produces implementation plans |
| `@research` | Explores technologies, compares options, and provides evidence-based recommendations |
| `@git` | Manages git workflows, commits, branches, PRs, and changelogs |
| `@devops` | Handles CI/CD pipelines, Docker, deployment configs, and infrastructure |
| `@release` | Coordinates releases — changelog, versioning, tagging, and deployment |
| `@refactor` | Improves code structure, readability, and maintainability without changing behavior |
| `@performance` | Profiles, analyzes, and optimizes application performance |
| `@ask` | Asks clarifying questions to understand requirements before any work begins |
| `@cybersecurity` | Infrastructure security, network assessments, compliance audits, and incident response |
| `@hacking` | Offensive security testing, penetration testing, CTF challenge solving, exploit development, and red team operations |

---

## Skills

Skills are domain-specific knowledge packs loaded on demand. They complement agents by providing specialized workflows, patterns, and best practices.

| Skill | Description |
|-------|-------------|
| **api-docs** | Generate API documentation from existing code — OpenAPI specs, README sections, SDK docs |
| **api-scaffold** | Generate API endpoints, routes, and handlers from specifications |
| **auth** | Design and implement authentication and authorization systems (JWT, OAuth, RBAC) |
| **ci-pipeline** | Generate CI/CD pipeline configurations for GitHub Actions, GitLab CI, CircleCI |
| **database** | Handle migrations, seeding, schema changes, and test data generation |
| **dep-audit** | Check for outdated, vulnerable, or problematic dependencies |
| **dev-env** | Set up dev tooling, linting, formatting, monorepo structure, and project scaffolding |
| **dockerize** | Create Dockerfiles, docker-compose configs, and containerization setups |
| **frontend** | Build UI components, pages, and frontend architecture (React, Vue, Svelte, Angular) |
| **git-workflow** | Manage branching strategies, PR automation, changelogs, and team collaboration |
| **performance** | Profile, analyze, and optimize backend, frontend, database, and build times |
| **testing** | Design testing strategies, write test plans, and generate comprehensive test suites |

---

## Getting Started

Clone this repository into OpenCode's configuration directory:

```bash
git clone https://github.com/medispis/opencode-config.git ~/.config/opencode
```

Once cloned, install the dependencies and you're ready to use all agents and skills.

---

## Requirements

- **Node.js** >= 18.0.0
- **bun** (recommended) or **npm** as package manager

---

## Quick Install

```bash
cd ~/.config/opencode && bun install
```

Or using npm:

```bash
cd ~/.config/opencode && npm install
```

---

## Project Structure

```
~/.config/opencode/
├── opencode.json              # Main configuration file
├── AGENTS.md                  # Global rules and agent definitions
├── CHANGELOG.md               # Version history
├── README.md                  # This file
├── agents/                    # Agent definition files
│   ├── ask.md
│   ├── build.md
│   ├── debug.md
│   ├── devops.md
│   ├── docs.md
│   ├── git.md
│   ├── hacking.md
│   ├── cybersecurity.md
│   ├── orchestrator.md
│   ├── performance.md
│   ├── plan.md
│   ├── refactor.md
│   ├── release.md
│   ├── research.md
│   ├── review.md
│   ├── security.md
│   └── test.md
├── skills/                    # Skill definitions
│   ├── api-docs/
│   ├── api-scaffold/
│   ├── auth/
│   ├── ci-pipeline/
│   ├── database/
│   ├── dep-audit/
│   ├── dev-env/
│   ├── dockerize/
│   ├── frontend/
│   ├── git-workflow/
│   ├── performance/
│   └── testing/
└── package.json               # Dependencies
```

---

## Contributing

Contributions are welcome! If you'd like to improve an agent, add a new skill, or fix a bug:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

Please ensure your changes follow the existing conventions and style.

---

Built with OpenCode
