---
description: Run parallel quality and security reviews on the current scope
agent: orchestrator
subtask: true
---

Review this scope: $ARGUMENTS
Depth: $1 (use standard if $1 is empty; accept quick, standard, or thorough)

Rules:
- Dispatch @quality and @defensive-security in parallel. Wait for both results.
- If $ARGUMENTS is empty, review the current uncommitted changes.
- Pass the depth value to both agents.
- Summarize both reports in one result. List blockers first.

Scope to review:

!`git diff --stat`
!`git log --oneline -5`

Dispatch 1 — quality:
- Check correctness, tests, style, and types.
- Depth quick means changed lines only. Depth standard means changed files. Depth thorough means changed files plus related files.

Dispatch 2 — defensive-security:
- Check input validation, auth, secrets exposure, and injection.
- Apply the same depth rule.

Return a combined report: verdict (pass or fail), findings ordered by severity, and fix actions per finding.
