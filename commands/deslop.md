---
description: Clean up code without changing behavior, then verify
agent: build
subtask: true
---

Clean up this scope: $ARGUMENTS (if $ARGUMENTS is empty, use changed files below).

Rules:
- Preserve behavior exactly. Change no logic, no API, and no output.
- Allow only: rename for clarity, remove dead code, simplify nesting, remove duplication, add missing types.
- Change one pattern at a time. Keep the diff small.
- Make no feature changes. Make no dependency changes.
- After cleanup, chain @quality in verify mode to confirm tests still pass.

Files in scope:

!`git diff --name-only`
!`git status --short`

Scope: $ARGUMENTS

Execute the cleanup, list each change with file and reason, then dispatch @quality to verify behavior is preserved.
