# J-Workflow

> Turn AI coding workflows into reusable prompts, engineering standards, and lessons learned.
>
> **Record the process. Extract the experience. Reuse the knowledge. Rebuild the project.**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/j-workflow)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-workflow/SKILL.md)

**English** | [中文](README.zh-CN.md)

## ✨ Overview

**J-Workflow** is a command-oriented workflow knowledge system for AI-assisted software development.

AI coding agents can write code quickly, but valuable development knowledge is often lost when a session ends: requirements, architectural decisions, failed attempts, debugging experience, user corrections, and successful solutions disappear with the conversation.

J-Workflow turns that development process into a **persistent, reusable workflow**. It is explicitly activated by a user command, so it does not interfere with normal coding unless you ask it to.

## 🚀 Command-Oriented Skill

J-Workflow uses a namespaced command concept similar to OpenSpec-style AI workflows:

| User-facing command | Description |
|---|---|
| `/j-workflow:recorder <name>` | Create or resume a persistent workflow and record the current development session |
| `/j-workflow:show` | List and inspect recorded workflows |
| `/j-workflow:prompt <name>` | Generate a complete project-reproduction prompt from workflow history |
| `/j-workflow:standard [name]` | Extract engineering standards and lessons learned |
| `/j-workflow:stop` | Temporarily stop recording for the current coding context without deleting history |

J-Workflow is **not automatically activated for every coding task**. The user explicitly invokes a workflow operation when recording or workflow knowledge operations are wanted.

### Important compatibility note

The `/j-workflow:...` syntax is a **user-facing namespace convention**. The portable Agent Skills format defines `SKILL.md` files and skill discovery, but it does not itself standardize a colon-style slash-command registry.

Therefore, the exact command shown by an AI coding agent may differ:

| Agent / host style | Typical exposure |
|---|---|
| Hosts with namespaced slash commands | `/j-workflow:recorder` |
| Hosts with flat slash commands | `/j-workflow-recorder` or an equivalent host-specific command |
| Hosts exposing skills by name | `j-workflow-recorder` |
| Hosts without custom command discovery | Explicitly invoke/load the corresponding `SKILL.md` |

The repository provides standard `SKILL.md` metadata for the root skill and every subcommand skill. This makes the **skill content portable**, while command registration remains host-specific.

This distinction is intentional: J-Workflow does not pretend that one slash-command syntax works identically in every coding agent. OpenSpec documents the same cross-tool difference: some hosts expose `/opsx:...`, while others use dash-style commands or named skills. citeturn0search0

## 🔄 Persistent Workflows

A **Workflow** represents one long-running development effort. A workflow contains multiple **Sessions**, and sessions contain meaningful development events.

Start a new workflow:

```text
/j-workflow:recorder my-project
```

The first working session creates:

```text
my-project
└── Session 001
```

Resume the same workflow later with the same command:

```text
/j-workflow:recorder my-project
```

J-Workflow detects the existing workflow, restores its latest state, and creates the next session:

```text
my-project
├── Session 001
├── Session 002
└── Session 003
```

Previous sessions are preserved. Workflow history is append-only.

### Stop Recording

When you temporarily do not want development activity to be recorded, use:

```text
/j-workflow:stop
```

This stops recording for the **current coding context**. It does not delete, rewrite, complete, or archive the Workflow or its historical Sessions and Events.

To resume recording later:

```text
/j-workflow:recorder my-project
```

`stop` means **stop recording**, not **complete the Workflow**.

## 🧠 What Gets Recorded?

J-Workflow focuses on information that helps another AI agent understand **how and why** a project was built.

Typical records include:

- User requirements and corrections
- Project constraints
- Architecture and implementation decisions
- Files created, modified, or deleted
- Commands and tools used when their results matter
- Dependencies and configuration
- Test and validation results
- Failed approaches
- Error causes
- Fixes and workarounds
- Important assumptions
- Reusable lessons

Secrets such as passwords, API keys, access tokens, and private keys should never be recorded.

## 🧩 From Workflow to Prompt Engineering

```text
Development History
        │
        ▼
Requirements + Decisions
        │
        ▼
Architecture + Implementation
        │
        ▼
Failures + Solutions
        │
        ▼
Standards + Lessons
        │
        ▼
Complete Generation Prompt
```

`/j-workflow:prompt <name>` synthesizes the accumulated workflow instead of merely summarizing the latest conversation.

## 📚 Engineering Standards & Lessons Learned

`/j-workflow:standard [name]` extracts reusable engineering knowledge from a selected workflow or all workflows.

### Engineering Standards

Stable and actionable rules such as architecture boundaries, naming, testing, dependency management, API design, logging, error handling, security, project structure, and build/deployment practices.

### Lessons Learned

Evidence-based knowledge derived from actual development problems:

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

A one-off workaround is not promoted to a universal standard without evidence that it generalizes.

## 📁 Project Structure

```text
j-workflow/
├── skills/
│   └── j-workflow/
│       ├── SKILL.md
│       ├── recorder/SKILL.md
│       ├── show/SKILL.md
│       ├── prompt/SKILL.md
│       ├── standard/SKILL.md
│       └── stop/SKILL.md
│
└── .j-recorder/
    ├── workflows/
    ├── prompts/
    ├── standards/
    ├── lessons/
    └── index.md
```

The workflow history is the **source of truth**. Prompts, standards, and lessons are derived artifacts.

## 🔧 Installation

J-Workflow is distributed as an AI Agent Skill.

Clone the repository:

```bash
git clone https://github.com/mhhyoucom2236/j-workflow.git
cd j-workflow
```

For an Agent Skills-compatible host, load the appropriate `SKILL.md`. The root skill describes the family; each subcommand has its own self-contained skill:

```text
skills/j-workflow/SKILL.md
skills/j-workflow/recorder/SKILL.md
skills/j-workflow/show/SKILL.md
skills/j-workflow/prompt/SKILL.md
skills/j-workflow/standard/SKILL.md
skills/j-workflow/stop/SKILL.md
```

For a host that supports namespaced slash commands, register the user-facing operations against the corresponding subcommand skills:

```text
/j-workflow:recorder  → skills/j-workflow/recorder/SKILL.md
/j-workflow:show      → skills/j-workflow/show/SKILL.md
/j-workflow:prompt    → skills/j-workflow/prompt/SKILL.md
/j-workflow:standard  → skills/j-workflow/standard/SKILL.md
/j-workflow:stop      → skills/j-workflow/stop/SKILL.md
```

Do not assume that a host will automatically create the colon-style commands merely because these directories exist. Command registration is a host integration concern.

## 💡 Quick Start

### 1. Start recording

```text
/j-workflow:recorder android-app
```

The agent creates or resumes the `android-app` workflow and begins recording meaningful events for the active coding context.

### 2. Develop normally

```text
Requirement
    ↓
Architecture Decision
    ↓
Code Change
    ↓
Build Failure
    ↓
Root Cause
    ↓
Fix
    ↓
Test
    ↓
User Correction
```

The recorder focuses on meaningful development knowledge rather than every trivial command or file operation.

### 3. Temporarily stop recording

```text
/j-workflow:stop
```

The agent continues normal coding, but J-Workflow does not append development events until recording is explicitly resumed.

### 4. Resume recording

```text
/j-workflow:recorder android-app
```

The existing workflow is resumed and a new Session is appended.

### 5. Inspect workflows

```text
/j-workflow:show
```

### 6. Generate a project prompt

```text
/j-workflow:prompt android-app
```

### 7. Extract knowledge

```text
/j-workflow:standard android-app
```

## 🎯 Design Principles

1. **Explicit activation** — J-Workflow is invoked by a user command rather than silently changing normal agent behavior.
2. **Explicit recording control** — recording can be stopped temporarily with `/j-workflow:stop` and resumed with the recorder command.
3. **Append-only history** — historical Sessions and Events are preserved.
4. **Source/derived separation** — workflow history is authoritative; generated artifacts are reproducible outputs.
5. **Facts over guesses** — unknown details remain unknown or explicitly inferred.
6. **Failures are knowledge** — failed approaches, root causes, and fixes are valuable development evidence.
7. **Secrets stay out** — credentials and private keys must never enter workflow history.

## 🤝 Contributing

Contributions are welcome. Please preserve deterministic behavior, append-only source history, evidence-based knowledge, reproducibility, and secret protection.

## 📄 License

MIT License. See [LICENSE](LICENSE).

## ⭐ Why J-Workflow?

AI coding agents are becoming increasingly capable. The next problem is not only **how to generate code**, but also **how to preserve and reuse the knowledge created while generating it**.

J-Workflow treats AI-assisted development history as a reusable engineering asset.

> **Don't let a successful AI coding session disappear when the session ends. Record it, learn from it, and make it reusable.**

## 🔗 Links

- Repository: https://github.com/mhhyoucom2236/j-workflow
- [J-Workflow Skill](skills/j-workflow/SKILL.md)
- [Recorder Skill](skills/j-workflow/recorder/SKILL.md)
- [Stop Skill](skills/j-workflow/stop/SKILL.md)
- [中文文档 / Chinese Documentation](README.zh-CN.md)
