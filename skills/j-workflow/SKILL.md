---
name: j-workflow
description: Command-oriented workflow knowledge system for AI coding agents. Use only when the user explicitly invokes a J-Workflow operation such as init, recorder, show, prompt, standard, or stop. It records development workflows and turns history into reusable prompts, engineering standards, lessons learned, and project specifications.
---

# J-Workflow

J-Workflow is a command-oriented AI coding skill for recording development workflows and turning accumulated history into reusable prompts, engineering standards, lessons learned, and project specifications.

## Command Namespace

J-Workflow is designed to expose these user-facing operations:

```text
/j-workflow:init
/j-workflow:recorder <workflow-name>
/j-workflow:show
/j-workflow:prompt <workflow-name>
/j-workflow:standard [workflow-name]
/j-workflow:stop
```

The `j-workflow` namespace identifies the skill family. The exact slash-command syntax is provided by the host agent; the behavior is defined by the corresponding subcommand skill in this repository.

## Commands

| Command | Purpose |
|---|---|
| `/j-workflow:init` | Inspect the current project and create/update its architecture, engineering conventions, code style, and environment specification |
| `/j-workflow:recorder <workflow-name>` | Create or resume a persistent workflow and record the current development session |
| `/j-workflow:show` | List recorded workflows and their status |
| `/j-workflow:prompt <workflow-name>` | Generate a self-contained project reproduction prompt from the workflow history |
| `/j-workflow:standard [workflow-name]` | Extract engineering standards and lessons learned |
| `/j-workflow:stop` | Stop recording the current workflow without deleting or changing historical records |

## Skill Selection Rule

The user explicitly invokes a J-Workflow operation. Do not automatically activate J-Workflow merely because a coding task is being performed.

When a J-Workflow operation is invoked:

1. Load the corresponding subcommand skill.
2. Follow that skill's procedure.
3. Use `.j-recorder/` as the workflow storage location.
4. Never expose or record secrets such as passwords, tokens, API keys, or private keys.

## Project Specification

`/j-workflow:init` creates:

```text
.j-recorder/project-spec.md
```

This specification captures the current project's architecture, module boundaries, environment versions, naming conventions, code style, testing conventions, and other implementation rules that can be verified from the repository.

`init` may ask the user to confirm unresolved decisions such as Android JDK/Gradle versions, architecture choices, or the preferred pattern for new modules when the repository contains ambiguity.

The specification is **passive by design**. J-Workflow must not automatically inject or enforce `project-spec.md` during ordinary coding. The user explicitly decides when an agent should read and use it.

`project-spec.md` is separate from `/j-workflow:standard` output:

- `project-spec.md` describes the current project's structure and implementation conventions.
- `engineering-standards.md` contains reusable standards extracted from workflow evidence.
- `lessons-learned.md` contains evidence-backed lessons from development history.

## Recording State

Recording is active only after `/j-workflow:recorder <workflow-name>` has been explicitly invoked. `/j-workflow:stop` disables recording for the current coding context.

Stopping recording does not delete the Workflow, Session, or Events. Historical records remain append-only. To resume recording, explicitly invoke `/j-workflow:recorder <workflow-name>` again.

## Storage

```text
.j-recorder/
├── workflows/
├── prompts/
├── standards/
├── lessons/
├── project-spec.md
└── index.md
```

`workflows/*.md` is the source of truth for development history. Prompts, standards, lessons, and the project specification are derived/reference artifacts.

## Compatibility

This repository uses the portable Agent Skills `SKILL.md` format. Hosts that discover Agent Skills can load the individual subcommand skills directly.

The `/j-workflow:...` form is a user-facing namespace convention, not a feature guaranteed by the SKILL.md format itself. Hosts differ in how they expose skills as slash commands. A host may expose the commands as `/j-workflow:init`, `/j-workflow:recorder`, `/j-workflow-recorder`, a named skill, or another equivalent form.

For hosts with native namespaced commands, map each command to the corresponding subcommand skill under this directory.

For hosts without namespaced commands, invoke the corresponding subcommand skill directly or use the host-specific command/skill adapter.
