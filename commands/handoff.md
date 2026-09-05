---
description: Write a compact session handoff under 40 lines
agent: docs
subtask: true
---

Write a handoff for audience: $ARGUMENTS (default is next-session if $ARGUMENTS is empty).

Rules:
- Keep the full handoff under 40 lines.
- State facts only. Include no new code and no new plans beyond next actions.
- Cover: goal, done, remaining, blockers, key files, next actions.
- Base the handoff on the context below plus the current conversation.

Session state:

!`git status --short`
!`git log --oneline -5`
!`git diff --stat`

Write the handoff now for: $ARGUMENTS
