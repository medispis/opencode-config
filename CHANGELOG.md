# OpenCode Configuration Changelog

All notable changes to the OpenCode agent configurations will be documented in this file.

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
