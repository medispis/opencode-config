---
description: Create a read-only implementation plan, change no code
agent: plan
subtask: true
---

Create an implementation plan. Make no code changes. Make no file edits.

Topic: $ARGUMENTS

Rules:
- $ARGUMENTS is required. If $ARGUMENTS is empty, stop and request the topic.
- Read files only. Run read-only commands only.
- Change no files. Run no builds that write output.

Include in the plan:
- Goal restated in one sentence.
- Files to read and files to change.
- Ordered steps (maximum 7 steps).
- Risks and open questions.
- Verification commands to run after implementation.

Use this context:

!`git status --short`
!`git log --oneline -5`

Return the plan and stop. Do not implement.
