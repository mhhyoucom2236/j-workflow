# J-Workflow

J-Workflow is a command-oriented AI coding skill for recording development workflows and turning accumulated history into reusable prompts, engineering standards, and lessons learned.

## Command Namespace

J-Workflow is designed to be invoked through namespaced skill commands:

```text
/j-workflow:recorder <workflow-name>
/j-workflow:show
/j-workflow:prompt <workflow-name>
/j-workflow:standard [workflow-name]
/j-workflow:stop
```

The `j-workflow` namespace selects the J-Workflow skill family. Each subcommand loads the behavior defined by its corresponding skill.

## Commands

| Command | Purpose |
|---|---|
| `/j-workflow:recorder <workflow-name>` | Create or resume a persistent workflow and record the current development session |
| `/j-workflow:show` | List recorded workflows and their status |
| `/j-workflow:prompt <workflow-name>` | Generate a self-contained project reproduction prompt from the workflow history |
| `/j-workflow:standard [workflow-name]` | Extract engineering standards and lessons learned |
| `/j-workflow:stop` | Stop recording the current workflow without deleting or changing its historical records |

## Skill Selection Rule

The user explicitly invokes a subcommand. Do not automatically activate J-Workflow merely because a coding task is being performed.

When a J-Workflow command is invoked:

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

For agents with native skill support, load this directory as the J-Workflow skill family.

For agents supporting namespaced slash commands, map:

```text
/j-workflow:recorder
/j-workflow:show
/j-workflow:prompt
/j-workflow:standard
/j-workflow:stop
```

to the corresponding subcommand skills under this directory.

For agents that do not support namespaced commands, invoke the corresponding subcommand skill directly.
