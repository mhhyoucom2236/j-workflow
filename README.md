# Cloud AI Note

> An open-source knowledge and workflow toolkit for AI-assisted software development.
>
> **Record the process. Extract the experience. Reuse the prompt. Rebuild the project.**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/cloud-ai-note)
[![Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-recorder/SKILL.md)

## ✨ Overview

**Cloud AI Note** is an open-source project focused on preserving the valuable knowledge produced during AI-assisted programming.

Most AI coding conversations are temporary: the agent solves a problem, the session ends, and the useful decisions, failed attempts, fixes, and lessons are lost.

Cloud AI Note introduces a persistent workflow recording mechanism that turns an AI coding session into reusable engineering knowledge.

```text
AI Coding Session
       │
       ▼
  Record Workflow
       │
       ▼
   Persistent History
       │
       ├───────────────┐
       ▼               ▼
Generate Prompt   Extract Knowledge
       │               │
       ▼               ├── Engineering Standards
Rebuild Project       └── Lessons Learned
```

## 🚀 J-Recorder

The first core capability of this project is **J-Recorder**, an agent-agnostic Skill for recording AI-assisted development workflows.

It is designed to work with modern coding agents that support Skills, slash commands, or equivalent instruction mechanisms.

### Core commands

| Command | Description |
|---|---|
| `/j-recorder-workflow <name>` | Create or resume a persistent development workflow |
| `/j-show-workflow` | List all recorded workflows |
| `/j-generate-prompt` | Generate a complete project-reproduction prompt |
| `/j-generate-standard` | Extract engineering standards and lessons learned |

### Persistent workflows

A workflow is not limited to a single AI session or a single day.

```text
/j-recorder-workflow my-project
```

First day:

```text
my-project
└── Session 001
```

Next day, run the same command:

```text
/j-recorder-workflow my-project
```

The existing workflow is resumed instead of creating a new one:

```text
my-project
├── Session 001
└── Session 002
```

The history is append-only. Previous development sessions are preserved.

## 🧠 From Development History to Prompt Engineering

J-Recorder does more than store a conversation.

It captures the information required to reproduce the development process:

- User requirements and corrections
- Architecture decisions
- Implementation choices
- File and directory changes
- Commands and tooling
- Dependencies and configuration
- Tests and validation results
- Failed approaches
- Root causes
- Fixes and workarounds
- Engineering constraints
- Reusable lessons

The accumulated workflow can then be transformed into a **self-contained project-generation prompt**.

The generated prompt is intended to contain enough information for another capable coding agent to build the project without reading the original workflow.

## 📚 Engineering Standards & Lessons Learned

Development history contains knowledge that is useful beyond one project.

`/j-generate-standard` extracts two categories:

### Engineering Standards

Stable and actionable development rules, such as:

- Architecture boundaries
- Naming conventions
- Testing practices
- Dependency management
- API design
- Error handling
- Logging
- Security practices
- Project structure
- Build and deployment conventions

### Lessons Learned

Experience derived from actual problems:

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

This makes the project's development history progressively more useful instead of becoming a static archive.

## 📁 Project Structure

```text
cloud-ai-note/
├── skills/
│   └── j-recorder/
│       └── SKILL.md
│
└── .j-recorder/
    ├── workflows/
    │   └── <workflow-name>.md
    ├── prompts/
    │   └── <workflow-name>.md
    ├── standards/
    │   └── engineering-standards.md
    ├── lessons/
    │   └── lessons-learned.md
    └── index.md
```

The workflow history is the source of truth. Generated prompts, standards, and lessons are derived artifacts.

## 🔧 Installation

J-Recorder is currently distributed as a Skill rather than a traditional application package.

Clone the repository:

```bash
git clone https://github.com/mhhyoucom2236/cloud-ai-note.git
cd cloud-ai-note
```

Then load the Skill according to the Skill mechanism supported by your coding agent:

```text
skills/j-recorder/SKILL.md
```

> Agent-specific installation instructions will be added as compatibility integrations are introduced.

## 💡 Example Workflow

Imagine you are developing an Android application with an AI coding agent.

### 1. Start recording

```text
/j-recorder-workflow android-app
```

### 2. Develop normally

The agent records meaningful events such as:

```text
Requirement
→ Architecture decision
→ Code change
→ Build failure
→ Root-cause analysis
→ Fix
→ Test
→ User correction
```

### 3. Continue tomorrow

```text
/j-recorder-workflow android-app
```

The agent resumes the existing workflow and starts the next Session.

### 4. Generate a reproduction prompt

```text
/j-generate-prompt
```

The complete development history is transformed into a reusable project-generation prompt.

### 5. Extract engineering knowledge

```text
/j-generate-standard
```

The workflow is analyzed to extract reusable standards and lessons learned.

## 🎯 Goals

Cloud AI Note aims to evolve toward a reusable knowledge layer for AI coding agents:

```text
                 ┌─────────────────────┐
                 │   AI Coding Agent   │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │   J-Recorder Skill  │
                 └──────────┬──────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │ Persistent Workflow │
                 └──────────┬──────────┘
                            │
              ┌─────────────┴─────────────┐
              ▼                           ▼
      ┌───────────────┐           ┌───────────────┐
      │ Prompt Engine │           │  Knowledge    │
      │   Generation  │           │  Extraction   │
      └───────┬───────┘           └───────┬───────┘
              │                           │
              ▼                           ▼
       Project Rebuild            Standards / Lessons
```

Long-term goals include:

- Support for mainstream coding agents
- Better workflow summarization and reconstruction
- Automatic knowledge extraction
- Cross-project engineering standards
- Reusable prompt libraries
- More robust workflow indexing and search
- Agent-specific integrations

## 🤝 Contributing

Contributions are welcome.

If you have ideas for improving workflow recording, prompt generation, knowledge extraction, or agent compatibility, feel free to open an Issue or Pull Request.

Before submitting a change, please keep the following principles in mind:

1. Preserve original workflow history.
2. Prefer deterministic and reproducible behavior.
3. Record facts instead of inventing information.
4. Treat failed approaches as valuable engineering evidence.
5. Keep generated artifacts derived from source workflow history.

## 📄 License

License information will be added as the project matures.

## ⭐ Why Cloud AI Note?

AI coding agents are becoming increasingly capable, but the development knowledge generated during a project is still easy to lose.

Cloud AI Note treats that knowledge as a reusable engineering asset:

> **Don't let a successful AI coding session disappear when the session ends. Record it, learn from it, and make it reusable.**
