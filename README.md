# OpenCode Agent Configuration

Personal configuration for OpenCode AI — a multi-agent system with specialized agents and skills for software development workflows.

[![GitHub](https://img.shields.io/badge/GitHub-medispis/opencode--config-181717?style=flat-square&logo=github)](https://github.com/medispis/opencode-config)

---

## Table of Contents

- [Agents](#agents)
- [Skills](#skills)
- [Commands](#commands)
- [Workflow](#workflow)
- [Getting Started](#getting-started)
- [Requirements](#requirements)
- [Quick Install](#quick-install)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

---

## Agents

This configuration defines **9 specialized agents**, each tuned for a specific domain in the software development lifecycle. Agents are invoked via `@name` and have dedicated temperature settings optimized for their role.

Follow the standard pipeline: plan → build → quality + defensive-security → docs / delivery.

| Agent | Purpose |
|-------|---------|
| `@orchestrator` | Breaks down complex tasks and delegates to specialists |
| `@ask` | Asks clarifying questions to understand requirements before any work begins |
| `@plan` | Designs technical architecture and produces implementation plans |
| `@build` | Implements features, writes code, and safely refactors existing code |
| `@quality` | Diagnoses bugs, verifies code with tests, gates quality with reviews |
| `@defensive-security` | Defensive security audit and hardening — code, dependencies, infra, CIS/NIST compliance (no exploitation) |
| `@hacking` | Offensive security testing, penetration testing, CTF challenge solving, exploit development, and red team operations |
| `@delivery` | Handles version control, releases, and delivery pipelines — git, semver, changelog, CI/Docker |
| `@docs` | Creates and maintains project documentation, READMEs, API docs, and code comments |

---

## Skills

Skills are domain-specific knowledge packs loaded on demand. They complement agents by providing specialized workflows, patterns, and best practices. This configuration includes **12 skills**.

| Skill | Description |
|-------|-------------|
| **api-scaffold** | Generate API endpoints, routes, and handlers from specifications |
| **auth** | Design and implement authentication and authorization systems (JWT, OAuth, RBAC) |
| **ci-pipeline** | Generate CI/CD pipeline configurations for GitHub Actions, GitLab CI, CircleCI |
| **database** | Handle migrations, seeding, schema changes, and test data generation |
| **dep-audit** | Check for outdated, vulnerable, or problematic dependencies |
| **dev-env** | Set up dev tooling, linting, formatting, monorepo structure, and project scaffolding |
| **dockerize** | Create Dockerfiles, docker-compose configs, and containerization setups |
| **frontend** | Build UI components, pages, and frontend architecture (React, Vue, Svelte, Angular) |
| **git-workflow** | Manage branching strategies, PR automation, changelogs, and team collaboration |
| **gpb-to-mxb** | Converts PiBoSo GPBikes mods to MX Bikes — bikes, tracks, tyres, helmets, suits, and other assets |
| **performance** | Profile, analyze, and optimize backend, frontend, database, and build times |
| **testing** | Design testing strategies, write test plans, and generate comprehensive test suites |

---

## Commands

This configuration includes **8 slash commands**. Type `/name` to run a command.

| Command | Purpose | Agent |
|---------|---------|-------|
| `/loop` | Loop plan-build-quality until DONE criteria pass, max 5 rounds | `@orchestrator` |
| `/plan` | Create a read-only implementation plan, change no code | `@plan` |
| `/review` | Run parallel quality and security reviews on the current scope | `@orchestrator` |
| `/rewind` | Save, list, or restore a work snapshot with git | `@delivery` |
| `/ask` | Clarify requirements with targeted questions before work starts | `@ask` |
| `/handoff` | Write a compact session handoff under 40 lines | `@docs` |
| `/deslop` | Clean up code without changing behavior, then verify | `@build` |
| `/ship` | Run pre-flight checks then commit, push only on request | `@delivery` |

Restart opencode to load new commands.

---

## Workflow

```
@ask → @plan → @build → @quality + @defensive-security → @docs / @delivery
                       ↳ @hacking (authorized offensive work only, separate from defensive audits)
```

- Start with `@ask` when requirements are unclear, then `@plan`, then `@build`.
- Run `@quality` and `@defensive-security` in parallel after each `@build` change.
- Finish with `@docs` or `@delivery`.
- Route security work by type: defensive audits to `@defensive-security`, exploitation and red-team work to `@hacking`.

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
├── agents/                    # Agent definition files (9 agents)
│   ├── ask.md
│   ├── build.md
│   ├── defensive-security.md
│   ├── delivery.md
│   ├── docs.md
│   ├── hacking.md
│   ├── orchestrator.md
│   ├── plan.md
│   └── quality.md
├── commands/                  # Slash command definitions (8 commands)
│   ├── ask.md
│   ├── deslop.md
│   ├── handoff.md
│   ├── loop.md
│   ├── plan.md
│   ├── review.md
│   ├── rewind.md
│   └── ship.md
├── skills/                    # Skill definitions (12 skills)
│   ├── api-scaffold/
│   ├── auth/
│   ├── ci-pipeline/
│   ├── database/
│   ├── dep-audit/
│   ├── dev-env/
│   ├── dockerize/
│   ├── frontend/
│   ├── git-workflow/
│   ├── gpb-to-mxb/
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
