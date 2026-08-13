---
name: j-workflow-standard
description: Extract reusable engineering standards and lessons learned from J-Workflow history. Use when the user explicitly asks to generate or update standards and lessons, for example /j-workflow:standard [workflow-name].
---

# J-Workflow Standard

Use this skill only when the user explicitly invokes:

```text
/j-workflow:standard [workflow-name]
```

## Purpose

Extract reusable engineering knowledge from a selected Workflow or from all available Workflow histories.

## Procedure

1. Select the specified Workflow, or inspect all workflows when no name is provided.
2. Read relevant Sessions and Events.
3. Separate evidence-backed reusable rules from one-off workarounds.
4. Generate or update:
   - `.j-recorder/standards/engineering-standards.md`
   - `.j-recorder/lessons/lessons-learned.md`
5. Keep Workflow history as the source of truth.
6. Never include secrets or private credentials.

## Engineering Standards

Extract stable, actionable rules for areas such as architecture, naming, testing, dependency management, API design, logging, error handling, security, project structure, and build/deployment.

Do not promote a rule to a general standard unless the recorded evidence supports it.

## Lessons Learned

For each meaningful lesson, prefer:

```text
Situation
↓
Failed Approach
↓
Root Cause
↓
Correct Approach
↓
Prevention Rule
```

Lessons should be traceable to recorded development evidence whenever practical.
