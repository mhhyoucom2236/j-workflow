# J-Recorder

J-Recorder is a portable, agent-agnostic coding skill for recording AI-assisted development and turning the accumulated process into reusable prompt engineering, engineering standards, and lessons learned.

## Core Concept

A **Workflow** is a long-lived development record. A Workflow contains multiple **Sessions**. A Session represents one continuous development period and normally corresponds to one working day or one resumed coding context.

The same workflow can be resumed on another day by using the exact same workflow name:

```text
/j-recorder-workflow my-project
```

If `my-project` already exists, never create a second workflow and never overwrite history. Load the existing workflow, inspect its latest state, create a new Session, and append new events.

The source workflow is append-only. Generated prompts, standards, and lessons are derived artifacts.

## Commands

### `/j-recorder-workflow <workflow-name>`

Start or resume a persistent workflow.

#### New workflow

If `.j-recorder/workflows/<workflow-name>.md` does not exist:

1. Create the workflow file.
2. Create Session 001.
3. Record the current project context and initial intent.
4. Continue recording the development process.

#### Existing workflow

If the workflow file already exists:

1. Load the complete workflow history, or at minimum its latest session plus accumulated decisions, lessons, and current state.
2. Determine the next session number from existing sessions.
3. Create a new Session section; do not modify or delete previous sessions.
4. Reconcile the previous recorded state with the current repository state.
5. Record any differences between the previous state and current state.
6. Continue appending development events to the new session.

Example:

```text
Day 1:
/j-recorder-workflow my-project

→ creates Session 001

Day 2:
/j-recorder-workflow my-project

→ finds my-project
→ loads previous history
→ creates Session 002
→ continues recording

Day 3:
/j-recorder-workflow my-project

→ creates Session 003
```

The user should never need to provide a new workflow name merely because development resumed on another day.

#### Recording requirements

Record meaningful:

- user requirements and corrections;
- architecture and implementation decisions;
- files created, modified, or deleted;
- commands executed;
- dependencies and configuration changes;
- tests and validation results;
- failures and error messages when relevant;
- root causes;
- fixes and workarounds;
- assumptions and unresolved questions;
- reusable lessons.

Never record secrets, tokens, passwords, private keys, or unrelated private information.

### `/j-show-workflow`

List all workflows found under `.j-recorder/workflows/`.

Return:

- workflow name;
- status;
- session count;
- event count when available;
- created date;
- last updated date.

### `/j-generate-prompt`

Generate a complete, self-contained prompt that another capable coding agent can use to reproduce the project from scratch.

The generated prompt must synthesize **all sessions** of the selected workflow. It must not simply summarize the latest session.

Include:

1. Role and execution rules for the coding agent.
2. Project goal and functional requirements.
3. Non-functional requirements.
4. Technology stack and version constraints.
5. Architecture and module boundaries.
6. Directory and file structure.
7. Data models and interfaces.
8. APIs, commands, integrations, and configuration.
9. Detailed implementation steps in dependency order.
10. Testing and validation requirements.
11. Error handling and edge cases.
12. Security constraints and forbidden practices.
13. Engineering standards extracted from the workflow.
14. Lessons learned and known failure modes.
15. Definition of done and acceptance criteria.

Resolve contradictions using the latest explicit user decision. Preserve late-discovered constraints. Convert failures into prevention instructions. Do not invent unknown details; label them as unknown or inferred.

Save generated output to:

```text
.j-recorder/prompts/<workflow-name>.md
```

### `/j-generate-standard`

Analyze the selected workflow or all workflows and extract reusable engineering knowledge.

Produce two primary sections.

#### Engineering Standards

Stable, actionable rules that should become project/team conventions.

Examples:

- architecture boundaries;
- naming conventions;
- testing rules;
- dependency management;
- API design;
- logging;
- error handling;
- security;
- file organization;
- build and deployment practices.

#### Lessons Learned

Knowledge derived from actual development problems. Each lesson should contain:

- Situation
- Failed or unsafe approach
- Root cause
- Correct approach
- Prevention rule
- Scope/applicability

Do not turn a one-off workaround into a universal standard without evidence that it generalizes.

Save generated artifacts to:

```text
.j-recorder/standards/engineering-standards.md
.j-recorder/lessons/lessons-learned.md
```

## Storage Layout

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

`workflows/*.md` is the source of truth. Generated artifacts must never replace source history.

## Workflow File Format

Every workflow should use this structure:

```text
# Workflow: <workflow-name>

## Metadata
- Created: <date>
- Last Updated: <date>
- Status: active|completed|paused
- Session Count: <number>

## Current State
- Goal: ...
- Current implementation: ...
- Known constraints: ...
- Open issues: ...
- Next recommended action: ...

---

# Session 001

- Started: <timestamp>
- Ended: <timestamp when known>

## Context
...

## Events

### Event 001
#### Intent
...
#### Action
...
#### Artifacts
...
#### Result
...
#### Problem
...
#### Root Cause
...
#### Resolution
...
#### Lesson
...

---

# Session 002
...
```

The metadata and Current State may be updated as a controlled index of the workflow, but historical Session content and Events are append-only. When updating Current State, do not rewrite historical facts.

## Session Rules

A Session starts whenever `/j-recorder-workflow <workflow-name>` resumes an existing workflow in a new coding context.

Do not create a new workflow merely because:

- the calendar date changed;
- the terminal was closed;
- the coding agent restarted;
- the user changed computers;
- the project was paused and resumed.

Always reuse the workflow name when the work belongs to the same project/workstream.

Create a new workflow only when the user explicitly gives a different workflow name or clearly starts an independent project.

## Recording Rules

- Prefer facts over interpretation.
- Preserve important decision order.
- Record why a decision was made when it affects reproducibility.
- Record failed attempts, not only successful attempts.
- Capture user corrections because they are high-value requirements.
- Capture validation evidence such as commands and results.
- Distinguish requirements from implementation choices.
- Mark uncertain assumptions explicitly.
- Deduplicate only during generation; never delete source history.
- Never invent missing implementation details.

## Resume Rules

When resuming a workflow, the agent should first establish continuity:

```text
Existing Workflow
      ↓
Load latest Current State
      ↓
Load latest Session
      ↓
Inspect current repository state
      ↓
Compare recorded state with actual state
      ↓
Record state changes if relevant
      ↓
Create new Session
      ↓
Continue development
```

The agent should not blindly assume that the repository is exactly where the previous session ended. It should inspect the actual project state when that is practical.

## Prompt Generation Rules

When generating a reproduction prompt:

- synthesize all Sessions;
- prioritize explicit user requirements;
- use the latest explicit decision when requirements conflict;
- preserve important constraints discovered late;
- promote repeated successful patterns into rules;
- convert failures into prevention instructions;
- include exact commands, paths, interfaces, schemas, and configuration when known;
- separate mandatory requirements from recommendations;
- make the final result usable as one pasted prompt;
- do not require the target agent to read the original workflow.

## Standard Extraction Rules

A candidate standard should generally be:

- repeated across events or clearly required by the project;
- stable rather than tied to a temporary bug;
- actionable and testable;
- specific enough to guide an agent.

A candidate lesson should have evidence of a problem, cause, and better approach. Assign a scope such as `project`, `stack`, `tool`, or `general`.

## Agent Behavior

When active, the coding agent should:

1. Determine whether the workflow exists.
2. Create or resume the appropriate Workflow.
3. Create a new Session when resuming an existing Workflow.
4. Inspect relevant history before major changes.
5. Perform coding work normally.
6. Append events after meaningful milestones, failures, fixes, and user corrections.
7. Keep source history append-only.
8. Validate changes with appropriate tests.
9. Update Current State without rewriting historical sessions.
10. Make generated artifacts reproducible from source history.
11. Never claim an inferred lesson is proven.

## Compatibility

For agents supporting native skills, load this `SKILL.md` as a skill.

For agents supporting slash commands, map:

```text
/j-recorder-workflow <workflow-name>
/j-show-workflow
/j-generate-prompt
/j-generate-standard
```

to the behaviors defined above.

For agents without slash-command support, the same operations can be requested in natural language after activating J-Recorder.
