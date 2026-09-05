# OpenCode Configuration Changelog

All notable changes to the OpenCode agent configurations will be documented in this file.

## [2026-09-05] - Slash Commands (8 commands)

### Added
- 8 slash commands in `commands/`: loop, plan, review, rewind, ask, handoff, deslop, ship
  - `/loop` (@orchestrator) - Loop plan-build-quality until DONE criteria pass, max 5 rounds
  - `/plan` (@plan) - Create a read-only implementation plan, change no code
  - `/review` (@orchestrator) - Run parallel quality and security reviews on the current scope
  - `/rewind` (@delivery) - Save, list, or restore a work snapshot with git
  - `/ask` (@ask) - Clarify requirements with targeted questions before work starts
  - `/handoff` (@docs) - Write a compact session handoff under 40 lines
  - `/deslop` (@build) - Clean up code without changing behavior, then verify
  - `/ship` (@delivery) - Run pre-flight checks then commit, push only on request
- Restart opencode to load new commands.

### Security
- Validated APPROVE by quality and defensive-security. No code changes needed, docs only.
- Future work (4 informational hardening ideas): ship push word-boundary check, $ARGUMENTS-as-data clause, rewind input validation, review input validation.

---

## [2026-09-05] - 9-Agent Consolidation

### Changed
- Consolidated from 17 agents to **9 agents**: orchestrator, ask, plan, build, quality, defensive-security, hacking, delivery, docs
  - `quality` absorbs debug, test, review, refactor, research, performance (bugs, tests, reviews, refactoring, research, optimization)
  - `defensive-security` absorbs security, cybersecurity (audits, hardening, compliance; no exploitation)
  - `delivery` absorbs git, devops, release (version control, CI/CD, Docker, releases, changelogs)
  - `build` absorbs feature implementation and safe refactoring
  - `hacking` unchanged (authorized offensive work only, separate from defensive audits)
- Standardized pipeline: plan → build → quality + defensive-security → docs / delivery
  - quality and defensive-security run in parallel after each build change before docs or delivery
  - Security work routed by type: defensive audits to defensive-security, exploitation to hacking
- **Token savings**: 8 fewer agent definition files loaded (~47% fewer agent files); fewer routing choices for orchestrator (9 targets instead of 17); parallel quality + defensive-security checks replace sequential debug → test → review → security chains
- **Skills**: 12 skills (api-docs deleted; api-scaffold, auth, ci-pipeline, database, dep-audit, dev-env, dockerize, frontend, git-workflow, gpb-to-mxb, performance, testing retained)

### Removed
- Deleted agents: debug, test, review, security, research, git, devops, release, refactor, performance, cybersecurity
- Deleted skill: api-docs

### Security
- No model field in any agent file (verified)
- Defensive-security has no exploitation permissions; hacking is isolated for authorized offensive work only

---

## [2026-03-23] - Major Configuration Improvements

### Added
- **build.md** - New dedicated agent for code implementation tasks
  - Handles feature development, code writing, and modifications
  - Temperature: 0.3 for balanced creativity
  - Full permissions for edit, write, bash, and webfetch
  - Includes comprehensive implementation methodology and quality standards

### Changed
- **AGENTS.md** - Enhanced global rules
  - Added "Agent Temperature Guidelines" section documenting the rationale for temperature values across agents
  - Added comprehensive "Error Handling" section with guidance for all error types
  - Updated subagent list to include new @build agent

- **agents/orchestrator.md**
  - Added @build agent to available subagents list
  - Updated workflow patterns to use @build instead of "implement"
  - Added `skill` permission to allow loading specialized skills

- **agents/git.md**
  - Added `gh *` command permission for GitHub CLI operations

- **agents/refactor.md**
  - Added test execution permissions for multiple test frameworks (npm, yarn, pnpm, bun, pytest, go, cargo, mvn, gradle)
  - Enables verification that tests pass after refactoring

- **agents/security.md**
  - Added audit tool permissions for dependency scanning (npm audit, yarn audit, pip-audit, cargo audit, go, bundle)
  - Added git show permission for examining commits

- **agents/ask.md**
  - Standardized `question` permission format to match other agents

- **agents/debug.md**
  - Added safety guardrails for bash operations
  - Emphasizes read-only commands and warnings before state modifications

- **agents/test.md**
  - Added guidance on reporting test failures clearly
  - Added recommendation to invoke @debug for complex test failures
  - Added safety note about only running test commands

- **agents/docs.md**
  - Added guidance on using webfetch to reference external documentation standards

### Fixed
- Standardized bash permission syntax across all agents to use consistent nested object format
- Clarified code implementation responsibility (now explicitly handled by @build agent)
- Resolved workflow ambiguity about which agent writes code

### Security
- No model declarations in any configuration files (verified)
- Maintained principle of least privilege for agent permissions
- Added explicit safety guardrails for agents with broad bash access

---

## Configuration Philosophy

This configuration follows these principles:

1. **Model-Agnostic**: No agent ever declares or references specific models
2. **Least Privilege**: Each agent has only the permissions needed for its role
3. **Clear Responsibilities**: Each agent has a well-defined scope without overlap
4. **Temperature by Purpose**: Agent creativity is tuned to task requirements
5. **Safety First**: Guardrails prevent destructive operations and emphasize verification
6. **Explicit Error Handling**: All agents follow consistent error reporting patterns
