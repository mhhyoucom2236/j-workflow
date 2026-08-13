# J-Workflow Recorder

Use this skill only when the user explicitly invokes:

```text
/j-workflow:recorder <workflow-name>
```

## Purpose

Create or resume a persistent development Workflow, create a Session, and record meaningful development events while the user continues coding.

## Procedure

### New Workflow

If `.j-recorder/workflows/<workflow-name>.md` does not exist:

1. Create the workflow file.
2. Create Session 001.
3. Record the current project context and initial intent.
4. Continue recording meaningful development events.

### Existing Workflow

If the workflow already exists:

1. Load its latest Current State and latest Session; inspect more history when needed.
2. Determine the next Session number.
3. Reconcile recorded state with the actual repository state when practical.
4. Record relevant differences.
5. Create a new Session without rewriting historical Sessions.
6. Continue recording the development process.

## Recording Requirements

Record meaningful:

- user requirements and corrections;
- architecture and implementation decisions;
- files created, modified, or deleted;
- commands executed when their results matter;
- dependencies and configuration changes;
- tests and validation results;
- failures and relevant error messages;
- root causes;
- fixes and workarounds;
- assumptions and unresolved questions;
- reusable lessons.

Do not record secrets, tokens, passwords, private keys, or unrelated private information.

## Workflow Format

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
```

Historical Sessions and Events are append-only. Current State may be updated as a controlled index.

## Resume Rules

A new Session starts each time the same Workflow is explicitly resumed in a new coding context. Do not create a new Workflow just because the date, terminal, agent process, or computer changed.

Always reuse the same workflow name for the same project/workstream.

## Behavior

After activation, the agent should work normally and append events at meaningful milestones, failures, fixes, and user corrections. It should not require the user to invoke a recorder command for every event.
