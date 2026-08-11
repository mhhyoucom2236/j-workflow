# J-Workflow

> Turn AI coding workflows into reusable prompts, engineering standards, and lessons learned.
>
> **Record the process. Extract the experience. Reuse the knowledge. Rebuild the project.**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/j-workflow)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-recorder/SKILL.md)

## ✨ Overview

**J-Workflow** is an open-source workflow knowledge system for AI-assisted software development.

AI coding agents can write code quickly, but valuable development knowledge is often lost when a session ends: requirements, architectural decisions, failed attempts, debugging experience, user corrections, and successful solutions disappear with the conversation.

J-Workflow solves this by turning the development process into a **persistent, reusable workflow**.

```text
AI Coding
   │
   ▼
Record Workflow
   │
   ▼
Persistent Sessions
   │
   ├─────────────────────┐
   ▼                     ▼
Generate Prompt     Extract Knowledge
   │                     │
   ▼                     ├── Engineering Standards
Rebuild Project          └── Lessons Learned
```

## 🚀 J-Recorder

**J-Recorder** is the first core Skill in J-Workflow.

It records meaningful AI-assisted development events and preserves them across coding sessions and working days. The same workflow can be resumed at any time without losing previous history.

### Core commands

| Command | Description |
|---|---|
| `/j-recorder-workflow <name>` | Create or resume a persistent workflow |
| `/j-show-workflow` | List all recorded workflows |
| `/j-generate-prompt` | Generate a complete project-reproduction prompt |
| `/j-generate-standard` | Extract engineering standards and lessons learned |

## 🔄 Persistent Workflows

A **Workflow** represents one long-running development effort. A workflow contains multiple **Sessions**.

Start a new workflow:

```text
/j-recorder-workflow my-project
```

The first working session creates:

```text
my-project
└── Session 001
```

The next day, simply run the same command:

```text
/j-recorder-workflow my-project
```

J-Recorder detects the existing workflow and continues it:

```text
my-project
├── Session 001
└── Session 002
```

Continue again later:

```text
my-project
├── Session 001
├── Session 002
└── Session 003
```

Previous sessions are preserved. The workflow history is append-only.

## 🧠 What Gets Recorded?

J-Recorder focuses on information that helps another AI agent understand **how and why** a project was built.

Typical records include:

- User requirements and corrections
- Project constraints
- Architecture decisions
- Implementation decisions
- Files created, modified, or deleted
- Commands and tools used
- Dependencies and configuration
- Test and validation results
- Failed approaches
- Error causes
- Fixes and workarounds
- Important assumptions
- Reusable lessons

Secrets such as passwords, API keys, access tokens, and private keys should never be recorded.

## 🧩 From Workflow to Prompt Engineering

The recorded workflow can be transformed into a **self-contained project-generation prompt**.

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

`/j-generate-prompt` synthesizes the accumulated workflow instead of merely summarizing the latest conversation.

The generated prompt is designed to contain enough concrete information for another capable coding agent to reproduce the project without reading the original workflow.

It can include:

- Project goals
- Functional requirements
- Non-functional requirements
- Technology stack
- Architecture
- Directory and file structure
- APIs and interfaces
- Data models
- Configuration
- Implementation steps
- Testing requirements
- Error handling
- Security constraints
- Engineering standards
- Known failure modes
- Acceptance criteria

## 📚 Engineering Standards & Lessons Learned

Development history contains reusable engineering knowledge.

`/j-generate-standard` extracts two categories of knowledge.

### Engineering Standards

Stable and actionable rules that can guide future development:

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

Lessons are derived from actual development problems rather than guesses.

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

This allows each development project to become a source of knowledge for future AI coding tasks.

## 📁 Project Structure

```text
j-workflow/
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

The workflow history is the **source of truth**. Prompts, standards, and lessons are derived artifacts.

## 🔧 Installation

J-Workflow is currently distributed as an AI Agent Skill.

Clone the repository:

```bash
git clone https://github.com/mhhyoucom2236/j-workflow.git
cd j-workflow
```

Load the Skill using the Skill mechanism supported by your coding agent:

```text
skills/j-recorder/SKILL.md
```

> Agent-specific installation and integration guides will be added as compatibility support expands.

## 💡 Example

Imagine you are developing an Android application with an AI coding agent.

### 1. Start recording

```text
/j-recorder-workflow android-app
```

### 2. Develop normally

The agent records meaningful events:

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

### 3. Continue tomorrow

Run the same workflow command:

```text
/j-recorder-workflow android-app
```

The existing workflow is resumed and a new Session is appended.

### 4. Generate a project prompt

```text
/j-generate-prompt
```

The complete workflow history is transformed into a reusable project-generation prompt.

### 5. Extract knowledge

```text
/j-generate-standard
```

Engineering standards and lessons learned are extracted from the recorded development experience.

## 🎯 Project Goals

J-Workflow aims to become a reusable knowledge layer for AI coding agents.

```text
                  ┌─────────────────────┐
                  │   AI Coding Agent   │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │    J-Workflow       │
                  │    J-Recorder       │
                  └──────────┬──────────┘
                             │
               ┌─────────────┴─────────────┐
               ▼                           ▼
       ┌────────────────┐          ┌────────────────┐
       │ Prompt Engine  │          │   Knowledge    │
       │  Generation    │          │   Extraction   │
       └───────┬────────┘          └───────┬────────┘
               │                           │
               ▼                           ▼
        Project Rebuild             Standards / Lessons
```

Long-term goals include:

- Support for mainstream AI coding agents
- Native Skill integrations
- Automatic workflow recording
- Better prompt reconstruction
- Cross-project engineering knowledge
- Reusable prompt libraries
- Workflow search and indexing
- Automatic lesson and standard propagation
- Agent-specific integrations

## 🤝 Contributing

Contributions are welcome.

If you have ideas for workflow recording, prompt generation, knowledge extraction, or AI agent compatibility, feel free to open an Issue or Pull Request.

Please keep these principles in mind:

1. Preserve original workflow history.
2. Prefer deterministic and reproducible behavior.
3. Record facts instead of inventing information.
4. Treat failed approaches as valuable engineering evidence.
5. Keep generated artifacts derived from workflow history.
6. Avoid recording secrets or private credentials.

## 📄 License

License information will be added as the project matures.

## ⭐ Why J-Workflow?

AI coding agents are becoming increasingly capable. The next problem is not only **how to generate code**, but also **how to preserve and reuse the knowledge created while generating it**.

J-Workflow treats AI-assisted development history as a reusable engineering asset.

> **Don't let a successful AI coding session disappear when the session ends. Record it, learn from it, and make it reusable.**

## 🔗 Links

- Repository: https://github.com/mhhyoucom2236/j-workflow
- J-Recorder Skill: [skills/j-recorder/SKILL.md](skills/j-recorder/SKILL.md)
