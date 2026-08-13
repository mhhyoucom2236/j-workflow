---
name: j-workflow
description: Command-oriented workflow knowledge system for AI coding agents. Use only when the user explicitly invokes a J-Workflow operation such as recorder, show, prompt, standard, or stop. It records development workflows and turns history into reusable prompts, engineering standards, and lessons learned.
---

# J-Workflow

J-Workflow is a command-oriented AI coding skill for recording development workflows and turning accumulated history into reusable prompts, engineering standards, and lessons learned.

## Command Namespace

J-Workflow is designed to expose these user-facing operations:

```text
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
└── index.md
```

`workflows/*.md` is the source of truth. Prompts, standards, and lessons are derived artifacts.

## Compatibility

This repository uses the portable Agent Skills `SKILL.md` format. Hosts that discover Agent Skills can load the individual subcommand skills directly.

The `/j-workflow:...` form is a user-facing namespace convention, not a feature guaranteed by the SKILL.md format itself. Hosts differ in how they expose skills as slash commands. A host may expose the commands as `/j-workflow:recorder`, `/j-workflow-recorder`, a named skill, or another equivalent form.

For hosts with native namespaced commands, map each command to the corresponding subcommand skill under this directory.

For hosts without namespaced commands, invoke the corresponding subcommand skill directly or use the host-specific command/skill adapter.
