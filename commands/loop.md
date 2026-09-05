---
description: Ralph loop - run plan-build-quality until DONE criteria pass, max 5 rounds
agent: orchestrator
subtask: false
---

Run this task to DONE. Do not stop after one pass.

Task: $ARGUMENTS

Rules:
- $ARGUMENTS must contain the task plus DONE criteria. If DONE criteria are missing, dispatch @ask first to define them.
- Loop this sequence until DONE criteria pass: @plan, then @build, then @quality.
- Run a maximum of 5 rounds. Stop after round 5 even if work is incomplete. Report what remains.
- After each @quality result, compare output against DONE criteria. Continue only if criteria fail.
- Keep each round scoped. Pass prior round results to the next round.

Round protocol:
1. Dispatch @plan to define or refine the next step.
2. Dispatch @build to implement that step.
3. Dispatch @quality to verify that step against DONE criteria.
4. Record pass or fail. Start the next round if work remains.

Stop conditions:
- All DONE criteria pass. Report completion and list changed files.
- Round 5 ends. Report status, passing criteria, failing criteria, and next actions.

Start round 1 now with: $ARGUMENTS
