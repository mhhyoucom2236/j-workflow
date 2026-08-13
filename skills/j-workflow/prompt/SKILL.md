# J-Workflow Prompt

Use this skill only when the user explicitly invokes:

```text
/j-workflow:prompt <workflow-name>
```

## Purpose

Generate a complete, self-contained prompt that another capable coding agent can use to reproduce the selected project from scratch.

## Procedure

1. Load the selected workflow's complete history, or all relevant Sessions when the history is large.
2. Synthesize requirements, decisions, implementation details, failures, fixes, standards, and lessons.
3. Prefer the latest explicit user decision when requirements conflict.
4. Preserve constraints discovered late in development.
5. Convert important failures into prevention instructions.
6. Never invent unknown implementation details; label them as unknown or inferred.
7. Save the result to:

```text
.j-recorder/prompts/<workflow-name>.md
```

## Required Prompt Content

1. Role and execution rules.
2. Project goal and functional requirements.
3. Non-functional requirements.
4. Technology stack and version constraints.
5. Architecture and module boundaries.
6. Directory and file structure.
7. Data models and interfaces.
8. APIs, commands, integrations, and configuration.
9. Implementation steps in dependency order.
10. Testing and validation requirements.
11. Error handling and edge cases.
12. Security constraints and forbidden practices.
13. Engineering standards extracted from the workflow.
14. Lessons learned and known failure modes.
15. Definition of done and acceptance criteria.

The generated prompt must be usable as one pasted prompt without requiring the target agent to read the original workflow.
