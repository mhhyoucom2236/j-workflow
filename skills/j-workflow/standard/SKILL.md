# J-Workflow Standard

Use this skill only when the user explicitly invokes:

```text
/j-workflow:standard [workflow-name]
```

## Purpose

Extract reusable engineering knowledge from one selected workflow or, when no workflow name is supplied, from all workflows.

## Procedure

1. Select the requested workflow, or all workflows when no name is supplied.
2. Analyze development events, decisions, failures, fixes, and repeated patterns.
3. Separate stable engineering standards from evidence-based lessons.
4. Do not turn a one-off workaround into a universal standard without evidence that it generalizes.
5. Save the generated artifacts to:

```text
.j-recorder/standards/engineering-standards.md
.j-recorder/lessons/lessons-learned.md
```

## Engineering Standards

Extract stable, actionable rules such as:

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

A candidate standard should generally be repeated, clearly required, stable, actionable, and testable.

## Lessons Learned

Each lesson should contain:

- Situation
- Failed or unsafe approach
- Root cause
- Correct approach
- Prevention rule
- Scope/applicability (`project`, `stack`, `tool`, or `general`)

A candidate lesson must have evidence of a problem, cause, and better approach.

## Source Integrity

Treat `.j-recorder/workflows/*.md` as the source of truth. Generated standards and lessons are derived artifacts and must never rewrite or delete workflow history.
