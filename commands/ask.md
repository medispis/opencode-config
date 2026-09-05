---
description: Clarify requirements with targeted questions before work starts
agent: ask
subtask: true
---

Clarify this topic: $ARGUMENTS

Rules:
- Use the question tool to ask questions. Ask no questions in plain text.
- Ask a maximum of 3-5 questions. Ask fewer if the topic is clear.
- Cover goal, scope, constraints, and DONE criteria.
- Offer a recommended option first in each question.
- Stop after answers arrive. Return a summary of decisions and stop.

If $ARGUMENTS is empty, ask what the user wants to clarify first.

Current context:

!`git status --short`

Start with the highest-priority unknowns for: $ARGUMENTS
