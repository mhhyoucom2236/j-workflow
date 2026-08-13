---
name: j-workflow-show
description: Inspect J-Workflow history and list recorded workflows with status and session information. Use when the user explicitly asks to show or inspect J-Workflow workflows, for example /j-workflow:show.
---

# J-Workflow Show

Use this skill only when the user explicitly invokes:

```text
/j-workflow:show
```

## Purpose

Inspect `.j-recorder/workflows/` and present the available workflows.

## Procedure

1. Find all workflow files under `.j-recorder/workflows/`.
2. Read metadata for each workflow.
3. Report, when available:
   - workflow name;
   - status;
   - session count;
   - event count;
   - created date;
   - last updated date.
4. Do not modify workflow history.

If no workflows exist, clearly report that no J-Workflow workflows have been recorded yet.
