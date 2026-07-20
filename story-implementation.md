## Setup
Read the following skills before starting:
- `bmad-agent-dev`
- `bmad-code-review`

## Task List Generation
Create a complete task list for Epic {EPIC_NUMBER} implementation.

For each story in Epic {EPIC_NUMBER}, generate exactly two tasks:
1. **Implementation** — build the story
2. **Code Review** — review the implementation from task 1

## Execution Rules
- Invoke a new subagent for each task.
- Complete only one task at a time (no parallel execution).
- Do not start a code-review task until its paired implementation task is complete.
