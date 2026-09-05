# Global OpenCode Rules

## General Principles

- Write clean, readable, maintainable code.
- Follow existing project conventions — don't introduce new patterns without reason.
- Support all languages and frameworks. Adapt to whatever the project uses.
- Never expose secrets, API keys, or credentials in code or logs.
- Prefer explicit over implicit. Clear variable names, clear logic.
- Keep changes minimal and focused on the task.
- Never mention or suggest what agent configuration any agent should use.
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
- Follow the standard pipeline: plan → build → quality + defensive-security → docs / delivery.
- Run quality and defensive-security after each build change before docs or delivery.
- Use Tab to switch between primary agents: Orchestrator → Plan → Build.

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
- **Follow the pipeline order** — start with @ask for unclear requirements, then @plan, then @build, then @quality and @defensive-security in parallel, then @docs or @delivery.
- **Run independent checks in parallel** — @quality and @defensive-security run at the same time after @build. @docs and @delivery run last.
- **Sequence dependent work** — @plan completes before @build starts. @build completes before @quality starts.
- **Provide full context** — when delegating, include all relevant file paths, requirements, and constraints.
- **Set explicit expectations** — tell each agent exactly what to deliver and when to stop.
- **Verify completion** — check that each specialist's output meets the requirements before proceeding.
- **Route security work by type** — defensive audit and hardening go to @defensive-security. Exploitation and red-team work go to @hacking.

## Agent Boundaries

Each agent should:
- **Stay in scope** — only perform tasks within your designated specialty.
- **Hand off promptly** — when encountering work outside your domain, delegate to the appropriate specialist.
- **Report clearly** — summarize what you did, what changed, and any issues encountered.
- **Ask before assuming** — if requirements are ambiguous, use @ask to clarify rather than guessing.
- **Stop when done** — don't continue working beyond the scope of your assigned task.

## Agent Temperature Guidelines

Different agents use different temperature values based on their purpose:
- **0.1** (Precision-critical): defensive-security — must be deterministic and precise
- **0.2** (Deterministic operations): quality, delivery — need consistency
- **0.3** (Balanced creativity): orchestrator, plan, build, docs — balance between precision and flexibility
- **0.4** (Creative problem-solving): ask, hacking — needs to explore possibilities and ask varied questions

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
| @orchestrator | `agents/orchestrator.md` | Orchestrates multi-agent workflows — breaks down tasks, dispatches subagents in parallel or sequence, and coordinates the full pipeline |
| @ask | `agents/ask.md` | Asks clarifying questions to understand requirements before any work begins |
| @plan | `agents/plan.md` | Designs technical architecture, researches options, and produces implementation plans before code is written |
| @build | `agents/build.md` | Implements features, writes code, and safely refactors existing code |
| @quality | `agents/quality.md` | Diagnoses bugs, verifies code with tests, gates quality with reviews |
| @defensive-security | `agents/defensive-security.md` | Defensive security audit and hardening — code, dependencies, infra, CIS/NIST compliance (no exploitation) |
| @hacking | `agents/hacking.md` | Offensive security testing, penetration testing, CTF challenge solving, exploit development, and red team operations |
| @delivery | `agents/delivery.md` | Handles version control, releases, and delivery pipelines — git, semver, changelog, CI/Docker |
| @docs | `agents/docs.md` | Creates and maintains project documentation, READMEs, API docs, and code comments |

### When to Use Each Agent

- **@orchestrator** — Use for complex multi-step tasks. It breaks down the request and dispatches specialists in the correct pipeline order.
- **@ask** — Use when requirements are unclear, ambiguous, or you need user input before proceeding.
- **@plan** — Use for design decisions, architecture planning, technology comparison, evidence-based recommendations, and multi-file implementation plans. Run before @build.
- **@build** — Use for implementing features, writing code, making direct code changes, and improving existing code structure without changing behavior.
- **@quality** — Use after @build to investigate failures, write and run tests, and gate changes with reviews. Blocks @docs and @delivery until checks pass.
- **@defensive-security** — Use after @build for vulnerability audits, dependency checks, hardening, and compliance evaluation. Runs in parallel with @quality. Never use for exploitation.
- **@hacking** — Use only for authorized offensive work: penetration testing, CTF challenges, exploit development, and red-team operations. Keep separate from defensive audits.
- **@delivery** — Use last for version control, branch and PR work, semver and changelogs, CI/CD pipelines, Docker builds, and deployment preparation.
- **@docs** — Use after @quality passes to create or update READMEs, API documentation, guides, and code comments.

## Skills

Skills are domain-specific knowledge packs loaded on demand. They complement agents by providing specialized workflows, patterns, and best practices.

### Available Skills

| Skill | Purpose | Use when |
|-------|---------|----------|
| **api-scaffold** | Generate API endpoints and handlers | Adding new API routes, scaffolding CRUD operations |
| **auth** | Authentication and authorization systems | Adding login, JWT, OAuth, RBAC, permissions |
| **ci-pipeline** | CI/CD pipeline configurations | Setting up GitHub Actions, GitLab CI, deploy workflows |
| **database** | Migrations, seeding, schema changes | Creating migrations, seed data, factories, fixtures |
| **dep-audit** | Dependency vulnerability and outdated checks | Auditing dependencies, planning upgrades |
| **dev-env** | Dev tooling, linting, monorepo setup | Initializing projects, adding ESLint/Prettier, monorepos |
| **dockerize** | Dockerfiles and docker-compose configs | Containerizing apps, optimizing Docker builds |
| **frontend** | UI components and frontend architecture | Building React/Vue/Svelte components, state management |
| **git-workflow** | Branching strategies and PR automation | Setting up PR templates, commit conventions, branch protection |
| **gpb-to-mxb** | Converts PiBoSo GPBikes mods to MX Bikes | Use when converting GPBikes mods to MX Bikes, porting bikes/tracks/tyres |
| **performance** | Profiling and optimization | Investigating slowness, reducing bundle size, optimizing queries |
| **testing** | Test strategy and comprehensive test suites | Planning test coverage, designing test architecture |

### Skill Loading Guidelines

- Skills are loaded automatically when relevant — no manual invocation needed
- Each skill provides detection patterns so the right one activates for the right task
- Skills reference each other when complementary (e.g., `dockerize` references `dev-env` for env vars)
- If a task spans multiple skills, load the primary one and reference others as needed
