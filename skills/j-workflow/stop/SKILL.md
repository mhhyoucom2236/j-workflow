# J-Workflow Stop

Use this skill only when the user explicitly invokes:

```text
/j-workflow:stop
```

## Purpose

Stop J-Workflow recording for the current coding context.

This is a control command. It does not delete, rewrite, archive, or otherwise modify historical Workflow records.

## Behavior

When invoked:

1. Disable J-Workflow event recording for the current coding context.
2. Stop appending new development events to the active Workflow.
3. Keep all existing Workflow, Session, Event, Prompt, Standard, and Lesson files unchanged.
4. Do not create a new Session.
5. Do not delete or mark the Workflow as completed.
6. Do not record ordinary coding activity while recording is stopped.

The agent may continue normal coding work after stopping J-Workflow. J-Workflow remains inactive until the user explicitly invokes:

```text
/j-workflow:recorder <workflow-name>
```

## Important Distinction

`/j-workflow:stop` means **stop recording**, not **complete the Workflow**.

The current Workflow can be resumed later with the recorder command. Historical Sessions and Events remain append-only.

## Response

After successfully stopping recording, briefly confirm:

```text
J-Workflow recording stopped for the current coding context.

No historical records were deleted or modified.
To resume recording:
/j-workflow:recorder <workflow-name>
```
