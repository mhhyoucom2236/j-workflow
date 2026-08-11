# J-Recorder

J-Recorder is a portable coding-agent skill for recording AI-assisted development workflows and turning successful work into reusable prompt engineering, engineering standards, and lessons learned.

## Purpose

Record the development process instead of only the final code. A recorded workflow can later be transformed into:

1. A complete one-shot project generation prompt.
2. Engineering standards and conventions discovered during implementation.
3. Lessons learned, failed approaches, root causes, and prevention rules.
4. A reusable workflow that can be applied to another project or coding agent.

The skill is designed to be agent-agnostic. The slash commands below are logical commands; an agent integration may expose them directly or map them to its own command/skill mechanism.

## Commands

### `/j-recorder-workflow <workflow-name>`

Start or continue recording a workflow.

- If the workflow does not exist, create it.
- If it exists, append the current development event.
- Never silently overwrite previous events.
- Record meaningful AI actions, decisions, file changes, commands, test results, failures, fixes, and user corrections.
- Do not record secrets, access tokens, passwords, private keys, or unrelated personal data.

Recommended event format:

```text
## Event: <timestamp or sequence>

### Intent
What the user/agent wanted to achieve.

### Context
Relevant project state, files, constraints, and assumptions.

### Action
What the agent actually did.

### Artifacts
Files created/changed, commands executed, APIs/models/tools used.

### Result
What happened, including tests and validation.

### Problem
What failed or was suboptimal.

### Root Cause
Why it happened.

### Resolution
How it was fixed.

### Lesson
What should be remembered next time.
```

### `/j-show-workflow`

List all recorded workflow names.

Return a concise table containing:

- workflow name
- status
- event count
- created time
- last updated time

### `/j-generate-prompt`

Generate a complete, self-contained prompt that can reproduce the recorded project from scratch.

The generated prompt must be implementation-oriented rather than a summary. It should contain:

1. Role and execution rules for the coding agent.
2. Project goal and functional requirements.
3. Non-functional requirements.
4. Technology stack and version constraints when known.
5. Architecture and module boundaries.
6. Directory/file structure.
7. Data models and interfaces.
8. APIs, commands, integrations, and configuration.
9. Detailed implementation steps in dependency order.
10. Testing and validation requirements.
11. Error handling and edge cases.
12. Security constraints and forbidden practices.
13. Engineering standards extracted from the workflow.
14. Lessons learned and known failure modes.
15. Definition of done and acceptance criteria.

The prompt must contain enough concrete information for another capable coding agent to implement the project without reading the original workflow.

### `/j-generate-standard`

Analyze one or more workflows and extract reusable engineering knowledge.

Produce two separate sections:

#### Engineering Standards
Stable rules that should become project/team conventions, such as naming, architecture, testing, dependency management, API design, logging, error handling, and file organization.

#### Lessons Learned
Experience derived from actual problems. Each lesson should include:

- Situation
- Failed/unsafe approach
- Root cause
- Correct approach
- Prevention rule
- Scope/applicability

Do not turn a one-off workaround into a universal standard unless there is evidence that it generalizes.

## Storage Layout

Use a human-readable Markdown/JSONL hybrid-friendly layout. The default layout is:

```text
.j-recorder/
├── workflows/
│   ├── <workflow-name>.md
│   └── ...
├── prompts/
│   ├── <workflow-name>.md
│   └── ...
├── standards/
│   └── engineering-standards.md
├── lessons/
│   └── lessons-learned.md
└── index.md
```

`workflows/*.md` is the source of truth. Generated prompts and standards are derived artifacts and must never replace the original workflow record.

## Recording Rules

- Prefer facts over interpretation.
- Preserve the sequence of important decisions.
- Record why a decision was made when that reason affects reproducibility.
- Record failed attempts, not only successful attempts.
- Capture user corrections because they are high-value requirements.
- Capture validation evidence such as test commands and results.
- Distinguish requirements from implementation choices.
- Mark uncertain assumptions explicitly.
- Deduplicate repeated events during generation, but never delete source history.
- Never invent missing implementation details. Mark them as unknown or inferred.

## Prompt Generation Rules

When generating a reproduction prompt:

- Convert the workflow timeline into a deterministic implementation plan.
- Resolve contradictions using the latest explicit user decision.
- Preserve important constraints even if they were discovered late.
- Promote repeated successful patterns into explicit rules.
- Include failures as prevention instructions rather than reproducing the failures.
- Include exact commands, file paths, interfaces, schemas, and configuration when available.
- Separate mandatory requirements from recommendations.
- Make the final prompt usable as a single pasted instruction to a coding agent.

## Standard Extraction Rules

A candidate standard should generally be:

- repeated across multiple events or clearly required by the project;
- stable rather than tied to a temporary bug;
- actionable and testable;
- specific enough to guide an agent.

A candidate lesson should generally have evidence of a problem, its cause, and a better approach. Assign a scope such as `project`, `stack`, `tool`, or `general`.

## Agent Behavior

When this skill is active, the coding agent should:

1. Inspect the existing workflow before making major changes when recording is enabled.
2. Perform the requested coding work normally.
3. Append a workflow event after meaningful milestones, failures, fixes, or user corrections.
4. Keep source workflow history append-only.
5. Run tests/validation appropriate to the change.
6. Make generated prompt/standard artifacts reproducible from the source workflow.
7. Avoid claiming that a lesson is proven when it is only an assumption.

## Compatibility

For agents that support native skills, load this `SKILL.md` as a skill.
For agents that support slash commands, map the four commands to the command definitions in this document.
For agents without either mechanism, the same behavior can be invoked by asking the agent to "activate J-Recorder" and specifying the desired command.
