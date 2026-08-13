---
name: j-workflow-prompt
description: Generate a self-contained project reproduction prompt from J-Workflow history. Use when the user explicitly asks to generate a prompt from a workflow, for example /j-workflow:prompt <workflow-name>.
---

# J-Workflow Prompt

Use this skill only when the user explicitly invokes:

```text
/j-workflow:prompt <workflow-name>
```

## Purpose

Transform the accumulated Workflow history into a self-contained prompt that another AI coding agent can use to understand and reproduce the project or continue development.

## Procedure

1. Read the selected Workflow from `.j-recorder/workflows/`.
2. Read relevant Sessions and Events.
3. Preserve facts, constraints, decisions, failures, fixes, and unresolved issues.
4. Avoid inventing missing project details.
5. Generate or update `.j-recorder/prompts/<workflow-name>.md`.
6. Keep the generated prompt derived from Workflow history; do not treat it as the source of truth.

## Prompt Content

When supported by the recorded evidence, include:

- project goal;
- functional and non-functional requirements;
- technology stack and version constraints;
- architecture and directory structure;
- APIs, interfaces, and data models;
- configuration;
- implementation sequence and dependencies;
- tests and validation;
- error handling and edge cases;
- security constraints;
- engineering standards;
- known failure modes and lessons;
- acceptance criteria.

Never include secrets or private credentials.
