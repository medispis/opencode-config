# Global OpenCode Rules

## General Principles

- Write clean, readable, maintainable code.
- Follow existing project conventions — don't introduce new patterns without reason.
- Support all languages and frameworks. Adapt to whatever the project uses.
- Never expose secrets, API keys, or credentials in code or logs.
- Prefer explicit over implicit. Clear variable names, clear logic.
- Keep changes minimal and focused on the task.
- Never mention or suggest what model any agent should use.
- **Think before acting** — understand the problem fully before implementing a solution.
- **Validate assumptions** — don't assume requirements; verify with the user or existing code.
- **Consider impact** — think about how changes affect other parts of the system.

## Code Style

- Match the project's existing style (indentation, naming, imports).
- Use the project's existing libraries and utilities — don't reinvent.
- Add types where available (TypeScript, Python type hints, Go interfaces).
- Handle errors explicitly. Don't swallow exceptions silently.
- Write self-documenting code. Add comments only for non-obvious logic.
- **Consistency over cleverness** — prefer straightforward solutions that others can understand.
- **Test your changes** — verify that your code works as expected before considering the task complete.

## ASD-STE100 (Simplified Technical English)

Apply ASD-STE100 principles to all technical writing — code, documentation, code comments, commit messages, and responses — whenever it is useful:
- Use approved/simplified vocabulary; one meaning per term.
- Use one term for one concept — do not use synonyms.
- Keep sentences short; one instruction/idea per sentence.
- Use active voice.
- Use simple, consistent technical terms; avoid jargon and ambiguity.
- Use the same word for the same thing throughout a document.
- Avoid unnecessary abbreviations and complex verb forms.

## Direct Writing Style

Apply direct writing principles to all technical writing — code, documentation, code comments, commit messages, and responses — whenever it is useful:
- Cut filler words and phrases that add no meaning: "just", "very", "basically", "in order to", "due to the fact that".
- State facts as facts. Do not hedge with "might", "possibly", "probably", "I think", "seems", or "kind of".
- Put the main point first. Do not start with throat-clearing phrases like "It is important to note that" or "It should be mentioned that".
- Use strong verbs. Do not turn verbs into nouns: "make a decision" → "decide", "conduct an analysis" → "analyze".
- Remove words that repeat the meaning of another word: "end result", "each and every", "future plans".
- Prefer concrete, specific words over vague ones. Give numbers and names instead of "good", "big", "several", "a lot".
- Give instructions as commands: "Run the tests", not "You should run the tests".
- Do not start with "There is", "There are", or "It is" when a direct subject exists.
- Do not soften statements with apologies or disclaimers: "I might be wrong", "Just a thought", "Not sure but".
- If you are not sure, say so directly — do not guess.

## Workflow

- Read and understand existing code before making changes.
- Run lint/typecheck/test commands after changes if they exist in the project.
- For complex tasks, use the orchestrator agent — it will delegate to specialists.
- Use Tab to switch between primary agents: Orchestrator → Build → Plan.

## Anti-Looping Rules

- **Set clear completion criteria** before starting any task — define what "done" looks like.
- **Track progress explicitly** — use TodoWrite for multi-step tasks to avoid revisiting completed work.
- **Limit retries** — if an operation fails twice with the same error, stop and investigate the root cause instead of retrying.
- **Avoid circular dependencies** — don't call the same agent repeatedly for the same subtask; if stuck, escalate to orchestrator.
- **Check for existing solutions** — search the codebase before implementing; don't recreate what already exists.
- **Time-box exploration** — if you can't find what you need in 3 search attempts, ask for clarification.

## Orchestrator Delegation

The orchestrator agent should:
- **Analyze task complexity** — simple tasks (single file, clear requirements) go directly to the appropriate specialist.
- **Break down complex tasks** — decompose multi-step work into discrete subtasks with clear boundaries.
- **Assign one agent per subtask** — each specialist handles their domain; don't mix responsibilities.
- **Provide full context** — when delegating, include all relevant file paths, requirements, and constraints.
- **Set explicit expectations** — tell each agent exactly what to deliver and when to stop.
- **Handle dependencies** — sequence dependent tasks; run independent tasks in parallel when possible.
- **Verify completion** — check that each specialist's output meets the requirements before proceeding.

## Agent Boundaries

Each agent should:
- **Stay in scope** — only perform tasks within your designated specialty.
- **Hand off promptly** — when encountering work outside your domain, delegate to the appropriate specialist.
- **Report clearly** — summarize what you did, what changed, and any issues encountered.
- **Ask before assuming** — if requirements are ambiguous, use @ask to clarify rather than guessing.
- **Stop when done** — don't continue working beyond the scope of your assigned task.

## Agent Temperature Guidelines

Different agents use different temperature values based on their purpose:
- **0.1** (Precision-critical): review, security — must be deterministic and precise
- **0.2** (Deterministic operations): debug, test, refactor, git, devops, release — need consistency
- **0.3** (Balanced creativity): orchestrator, build, docs, plan, research — balance between precision and flexibility
- **0.4** (Creative problem-solving): ask — needs to explore possibilities and ask varied questions

## Error Handling

- **Report errors clearly** — always include full context (file paths, error messages, stack traces)
- **Suggest next steps** — when operations fail, provide actionable guidance
- **Don't retry blindly** — understand the cause before attempting again
- **Escalate when stuck** — if unable to proceed, report to orchestrator with details
- **Distinguish error types**:
  - File not found → suggest creating it or verifying the path
  - Permission denied → explain what permissions are needed
  - Syntax errors → fix immediately
  - Network errors → check connectivity and retry once
  - Tool failures → verify tool is installed and version is compatible
- **Learn from failures** — document recurring issues and their solutions for future reference
- **Fail fast** — if a critical dependency is missing, stop immediately rather than continuing with partial functionality

## Subagents

Each agent is defined in its own file under `agents/`. The table below is a quick reference — see individual files for full specifications.

| Agent | File | Purpose |
|-------|------|---------|
| @ask | `agents/ask.md` | Requirements gathering — asks clarifying questions |
| @plan | `agents/plan.md` | Technical design & architecture — produces implementation plans |
| @research | `agents/research.md` | Technology exploration — compares libraries, evidence-based recommendations |
| @build | `agents/build.md` | Code implementation — writes features, modifies code |
| @cybersecurity | `agents/cybersecurity.md` | Infrastructure & network security — assesses systems, networks, policies, compliance |
| @hacking | `agents/hacking.md` | Offensive security & CTF — penetration testing, exploitation, red team operations |
| @review | `agents/review.md` | Code review — checks quality, security, best practices |
| @test | `agents/test.md` | Testing — writes and runs tests |
| @docs | `agents/docs.md` | Documentation — creates READMEs, API docs, guides |
| @security | `agents/security.md` | Security audit — finds vulnerabilities |
| @debug | `agents/debug.md` | Bug investigation — uses RIVAH framework |
| @refactor | `agents/refactor.md` | Code refactoring — improves structure without changing behavior |
| @git | `agents/git.md` | Git/PR workflows — commits, branches, pull requests |
| @release | `agents/release.md` | Release coordination — changelogs, versioning, tagging |
| @devops | `agents/devops.md` | CI/CD, Docker, infrastructure |
| @performance | `agents/performance.md` | Performance profiling and optimization |

### When to Use Each Agent

- **@ask** — Use when requirements are unclear, ambiguous, or you need user input before proceeding
- **@plan** — Use for complex tasks requiring design decisions, architecture planning, or multi-file changes
- **@research** — Use when comparing technologies, evaluating libraries, or needing evidence-based recommendations
- **@build** — Use for implementing features, writing code, or making direct code changes
- **@cybersecurity** — Use for infrastructure security assessments, network hardening, compliance audits, and defensive security operations
- **@hacking** — Use for penetration testing, CTF challenges, exploit development, red team operations, and offensive security assessments
- **@review** — Use after code changes to ensure quality, security, and adherence to standards
- **@test** — Use for writing tests, running test suites, or validating functionality
- **@docs** — Use for creating or updating documentation, READMEs, or API documentation
- **@security** — Use for security audits, vulnerability assessments, or security-focused code reviews
- **@debug** — Use when investigating bugs, errors, or unexpected behavior
- **@refactor** — Use for improving code structure, readability, or maintainability without changing functionality
- **@git** — Use for git operations, commit management, branch operations, or PR workflows
- **@release** — Use for version management, changelog generation, or release preparation
- **@devops** — Use for CI/CD pipelines, Docker configurations, or infrastructure management
- **@performance** — Use for profiling, bottleneck analysis, and optimization recommendations

## Skills

Skills are domain-specific knowledge packs loaded on demand. They complement agents by providing specialized workflows, patterns, and best practices.

### Available Skills

| Skill | Purpose | Use when |
|-------|---------|----------|
| **api-docs** | Generate API documentation from code | Documenting endpoints, generating OpenAPI specs |
| **api-scaffold** | Generate API endpoints and handlers | Adding new API routes, scaffolding CRUD operations |
| **auth** | Authentication and authorization systems | Adding login, JWT, OAuth, RBAC, permissions |
| **ci-pipeline** | CI/CD pipeline configurations | Setting up GitHub Actions, GitLab CI, deploy workflows |
| **database** | Migrations, seeding, schema changes | Creating migrations, seed data, factories, fixtures |
| **dep-audit** | Dependency vulnerability and outdated checks | Auditing dependencies, planning upgrades |
| **dev-env** | Dev tooling, linting, monorepo setup | Initializing projects, adding ESLint/Prettier, monorepos |
| **dockerize** | Dockerfiles and docker-compose configs | Containerizing apps, optimizing Docker builds |
| **frontend** | UI components and frontend architecture | Building React/Vue/Svelte components, state management |
| **git-workflow** | Branching strategies and PR automation | Setting up PR templates, commit conventions, branch protection |
| **performance** | Profiling and optimization | Investigating slowness, reducing bundle size, optimizing queries |
| **testing** | Test strategy and comprehensive test suites | Planning test coverage, designing test architecture |

### Skill Loading Guidelines

- Skills are loaded automatically when relevant — no manual invocation needed
- Each skill provides detection patterns so the right one activates for the right task
- Skills reference each other when complementary (e.g., `dockerize` references `dev-env` for env vars)
- If a task spans multiple skills, load the primary one and reference others as needed
