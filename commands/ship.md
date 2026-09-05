---
description: Run pre-flight checks then commit, push only on request
agent: delivery
subtask: false
---

Ship this request: $ARGUMENTS

Rules:
- Run @quality and @defensive-security in parallel first. Stop if either check fails.
- Write conventional commits (type: subject). Keep the subject under 72 chars.
- Commit only by default. Push only if $ARGUMENTS contains the word push.
- Never commit secrets, keys, tokens, or env files. Inspect the diff first.
- Never use --force or destructive flags.
- Use the git-workflow skill for branch, commit, and push steps.

Pre-flight state:

!`git status --short`
!`git diff --stat`
!`git log --oneline -5`

Steps:
1. Review the staged and unstaged changes.
2. Dispatch @quality and @defensive-security in parallel for pre-flight approval.
3. Stage intended files only. Write one conventional commit.
4. Push only if $ARGUMENTS contains push. Otherwise stop after commit and report the commit hash.
