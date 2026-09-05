---
description: Save, list, or restore a work snapshot with git
agent: delivery
subtask: false
---

Action: $1 (save, list, or restore; default is save)
Name: $2

Rules:
- Use the git-workflow skill for all git operations.
- Inspect state first before any save or restore.
- Never run force push or destructive reset without explicit user consent.
- For save: create a snapshot with the given name ($2) or a timestamp if $2 is empty. Include untracked files.
- For list: show saved snapshots only. Change nothing.
- For restore: require $2. Confirm the working tree is clean or has a save first. Restore that snapshot only.

Current state:

!`git status --short`
!`git log --oneline -5`

Execute the requested action ($1, default save) for snapshot name ($2) and report the result.
