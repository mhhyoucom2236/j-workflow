# J-Workflow

> Turn AI coding workflows into reusable prompts, project architecture specifications, engineering standards, and lessons learned.
>
> **Record the process. Understand the project. Reuse the knowledge. Build consistently.**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/j-workflow)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-workflow/SKILL.md)

**English** | [中文](README.zh-CN.md)

## ✨ Overview

**J-Workflow** is a command-oriented workflow knowledge system for AI-assisted software development.

AI coding agents can write code quickly, but valuable development knowledge is often lost when a session ends: requirements, architectural decisions, failed attempts, debugging experience, user corrections, and successful solutions disappear with the conversation.

J-Workflow turns that development process into **persistent, reusable workflow knowledge**. It is explicitly activated by a user command, so it does not interfere with normal coding unless you ask it to.

## ⚡ One-Shot Agent Deployment

The easiest way to install J-Workflow is to give the following prompt to your coding agent. The agent should inspect its own environment, determine how it manages Agent Skills, and install J-Workflow using the appropriate mechanism instead of assuming a specific vendor or directory layout.

> **Copy the entire prompt below and paste it into your AI coding agent.**

```text
Install and configure J-Workflow in the current AI coding agent environment.

Repository:
https://github.com/mhhyoucom2236/j-workflow

Requirements:

1. Inspect the current project and the coding agent environment first.
2. Determine which Agent Skills mechanism this agent supports (for example, a skills directory, project-local skills, user/global skills, or another documented mechanism).
3. Clone or otherwise obtain the J-Workflow repository from the URL above.
4. Install/load the J-Workflow skill family using the host agent's standard Skill installation mechanism. Do not invent a vendor-specific installation path if the host provides documented conventions.
5. Make sure these skills are available:
   - j-workflow
   - j-workflow-init
   - j-workflow-recorder
   - j-workflow-show
   - j-workflow-prompt
   - j-workflow-standard
   - j-workflow-stop
6. Read the root skill and the required subcommand SKILL.md files before finishing installation.
7. If the host supports namespaced slash commands, expose these operations using the host's supported command mechanism:
   /j-workflow:init
   /j-workflow:recorder <workflow-name>
   /j-workflow:show
   /j-workflow:prompt <workflow-name>
   /j-workflow:standard [workflow-name]
   /j-workflow:stop
8. If the host does not support colon-style namespaced commands, use the closest supported command/skill naming convention. Do not modify J-Workflow's behavior just to emulate an unsupported command syntax.
9. Verify that the installed Skill files are readable and that the commands/skills can be discovered by the agent.
10. Do not modify the user's application source code or project files except where required by the host's Skill installation mechanism.
11. Do not record, copy, or expose passwords, API keys, access tokens, private keys, or other secrets.

After installation, report:
- Where J-Workflow was installed.
- Which command/skill names are actually available in this agent.
- Whether the requested /j-workflow:* syntax is supported natively or mapped to another host-specific form.
- A minimal example showing how to start recording with a workflow named "my-project".
```

This deployment prompt is intentionally **agent-aware**: it asks the coding agent to detect its own Skill system instead of assuming that Codex, Claude Code, OpenCode, Cursor, or another host uses the same installation path.

## 🚀 Command-Oriented Skill

J-Workflow uses a namespaced command concept similar to OpenSpec-style AI workflows:

| User-facing command | Description |
|---|---|
| `/j-workflow:init` | Analyze the current project and create a reusable project architecture/specification document |
| `/j-workflow:recorder <name>` | Create or resume a persistent workflow and record the current development session |
| `/j-workflow:show` | List and inspect recorded workflows |
| `/j-workflow:prompt <name>` | Generate a complete project-reproduction prompt from workflow history |
| `/j-workflow:standard [name]` | Extract engineering standards and lessons learned |
| `/j-workflow:stop` | Temporarily stop recording for the current coding context without deleting history |

J-Workflow is **not automatically activated for every coding task**. The user explicitly invokes a workflow operation when recording or workflow knowledge operations are wanted.

## 🏗️ Project Architecture Initialization

`/j-workflow:init` is an **on-demand project analysis command**. Its purpose is different from `/j-workflow:standard`:

- `init` describes the **current project's architecture and implementation conventions**.
- `standard` extracts **reusable engineering standards and lessons learned** from workflow history.

Run:

```text
/j-workflow:init
```

The agent analyzes the current project and attempts to understand:

- Project and module structure
- Architecture and layer boundaries
- Module responsibilities and dependencies
- Naming conventions
- Package and file organization
- Code style and implementation patterns
- UI/state management patterns
- Repository/data/network patterns
- Dependency injection
- Error handling and logging
- Testing conventions
- Build and deployment configuration
- Existing project-specific conventions

For technology-specific environments, the agent should inspect relevant versions and configuration. For example, an Android project may require checking:

- JDK version
- Gradle version
- Android Gradle Plugin (AGP) version
- Kotlin version
- `compileSdk`
- `targetSdk`
- `minSdk`
- Java/Kotlin language level
- Build variants and product flavors
- Dependency management

### User confirmation for ambiguous decisions

`init` must not silently invent architectural rules.

If the project contains multiple patterns, incomplete configuration, or an important decision that cannot be determined reliably from the codebase, the agent should **ask the user for confirmation before finalizing the specification**.

For example:

```text
The project contains both MVVM and MVI implementations.
Which architecture should new modules use?

A. MVVM
B. MVI
C. Keep both and choose based on module type
```

The same applies to environment standards such as JDK, Gradle, AGP, Kotlin, SDK versions, or other technology-specific conventions when the expected standard is unclear.

### Generated project specification

After analysis and required confirmations, `init` generates:

```text
.j-recorder/project-spec.md
```

This document records the project's current architecture, environment, conventions, coding style, module rules, and confirmed decisions so that the information can be explicitly reused in future coding tasks.

The generated specification should distinguish between:

- **Confirmed** — directly verified from the project or explicitly confirmed by the user
- **Observed / Inferred** — strongly indicated by existing implementation but not explicitly confirmed
- **Needs confirmation** — unresolved decisions that require the user

### Important: `init` does not automatically enforce the specification

`project-spec.md` is **not a global instruction automatically injected into every future coding task**.

After initialization, the user decides when the specification should be used. For example:

```text
Read .j-recorder/project-spec.md and implement the new payment module according to the project's existing architecture and coding conventions.
```

This keeps J-Workflow non-invasive: running `init` creates project knowledge, but the user remains in control of when that knowledge becomes an instruction for the coding agent.

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
│       ├── init/SKILL.md
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
    ├── project-spec.md
    └── index.md
```

The workflow history is the **source of truth**. Prompts, standards, lessons, and project specifications are derived or confirmed artifacts.

## 🔧 Installation

J-Workflow is distributed as an AI Agent Skill.

### Recommended: use the deployment prompt

For most users, copy the prompt from [One-Shot Agent Deployment](#-one-shot-agent-deployment) into your coding agent. The agent can then inspect the host environment and perform the appropriate installation.

### Manual installation

Clone the repository:

```bash
git clone https://github.com/mhhyoucom2236/j-workflow.git
cd j-workflow
```

For an Agent Skills-compatible host, load the appropriate `SKILL.md`. The root skill describes the family; each subcommand has its own self-contained skill:

```text
skills/j-workflow/SKILL.md
skills/j-workflow/init/SKILL.md
skills/j-workflow/recorder/SKILL.md
skills/j-workflow/show/SKILL.md
skills/j-workflow/prompt/SKILL.md
skills/j-workflow/standard/SKILL.md
skills/j-workflow/stop/SKILL.md
```

For a host that supports namespaced slash commands, register the user-facing operations against the corresponding subcommand skills:

```text
/j-workflow:init      → skills/j-workflow/init/SKILL.md
/j-workflow:recorder  → skills/j-workflow/recorder/SKILL.md
/j-workflow:show      → skills/j-workflow/show/SKILL.md
/j-workflow:prompt    → skills/j-workflow/prompt/SKILL.md
/j-workflow:standard  → skills/j-workflow/standard/SKILL.md
/j-workflow:stop      → skills/j-workflow/stop/SKILL.md
```

Do not assume that a host will automatically create the colon-style commands merely because these directories exist. Command registration is a host integration concern.

## 💡 Quick Start

### 1. Initialize the project specification

If you want J-Workflow to understand an existing project's architecture and conventions first:

```text
/j-workflow:init
```

Review and answer any questions about ambiguous architecture or environment decisions. The result is saved to:

```text
.j-recorder/project-spec.md
```

The specification remains passive until you explicitly tell the coding agent to use it.

### 2. Start recording

```text
/j-workflow:recorder android-app
```

The agent creates or resumes the `android-app` workflow and begins recording meaningful events for the active coding context.

### 3. Develop normally

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

### 4. Temporarily stop recording

```text
/j-workflow:stop
```

The agent continues normal coding, but J-Workflow does not append development events until recording is explicitly resumed.

### 5. Resume recording

```text
/j-workflow:recorder android-app
```

The existing workflow is resumed and a new Session is appended.

### 6. Inspect workflows

```text
/j-workflow:show
```

### 7. Generate a project prompt

```text
/j-workflow:prompt android-app
```

### 8. Extract knowledge

```text
/j-workflow:standard android-app
```

## 🎯 Design Principles

1. **Explicit activation** — J-Workflow is invoked by a user command rather than silently changing normal agent behavior.
2. **Explicit project specification** — `/j-workflow:init` creates architecture knowledge without automatically enforcing it.
3. **Explicit recording control** — recording can be stopped temporarily with `/j-workflow:stop` and resumed with the recorder command.
4. **Append-only history** — historical Sessions and Events are preserved.
5. **Source/derived separation** — workflow history is authoritative; generated artifacts are reproducible or confirmed outputs.
6. **Facts over guesses** — unknown details remain unknown or explicitly inferred.
7. **User confirmation for decisions** — important ambiguous architecture and environment choices are confirmed instead of guessed.
8. **Failures are knowledge** — failed approaches, root causes, and fixes are valuable development evidence.
9. **Secrets stay out** — credentials and private keys must never enter workflow history or generated specifications.

## 🤝 Contributing

Contributions are welcome. Please preserve deterministic behavior, append-only source history, evidence-based knowledge, reproducibility, explicit activation, and secret protection.

## 📄 License

MIT License. See [LICENSE](LICENSE).

## ⭐ Why J-Workflow?

AI coding agents are becoming increasingly capable. The next problem is not only **how to generate code**, but also **how to preserve and reuse the knowledge created while generating it**.

J-Workflow treats AI-assisted development history and project architecture knowledge as reusable engineering assets.

> **Don't let a successful AI coding session disappear when the session ends. Record it, understand the project, and make the knowledge reusable.**

## 🔗 Links

- Repository: https://github.com/mhhyoucom2236/j-workflow
- [J-Workflow Skill](skills/j-workflow/SKILL.md)
- [Init Skill](skills/j-workflow/init/SKILL.md)
- [Recorder Skill](skills/j-workflow/recorder/SKILL.md)
- [Stop Skill](skills/j-workflow/stop/SKILL.md)
- [Chinese Documentation / 中文文档](README.zh-CN.md)
