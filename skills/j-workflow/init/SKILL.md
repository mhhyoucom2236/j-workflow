---
name: j-workflow-init
description: Inspect the current project to establish its architecture, engineering conventions, code style, and environment baseline. Use only when the user explicitly invokes /j-workflow:init. Ask the user to confirm unresolved project decisions before generating the project specification.
---

# J-Workflow Init

Use this skill only when the user explicitly invokes:

```text
/j-workflow:init
```

## Purpose

`init` creates a reusable project specification from the **actual current project**. It is intended to help future AI coding work produce modules that match the existing project's architecture, engineering conventions, environment, and code style.

The generated specification is a reference artifact for the user. **Do not automatically apply it to unrelated coding tasks, inject it into every future prompt, or tell the agent to follow it unless the user explicitly asks to use the specification.**

## Output

Generate or update:

```text
.j-recorder/project-spec.md
```

This file is the project's architecture and implementation specification. It is separate from workflow history and from `/j-workflow:standard` derived standards.

## Core Procedure

### 1. Inspect before asking

Inspect the current project and collect evidence from:

- Top-level directory structure
- Source/module/package structure
- Build and dependency configuration
- Existing architecture boundaries and layers
- Representative feature modules
- Naming conventions
- File and class organization
- UI/component patterns
- State management patterns
- Data/network/repository patterns
- Error handling and logging
- Dependency injection
- Testing conventions
- Resource organization
- Documentation and configuration files
- CI/build/deployment configuration when relevant

Prefer existing project evidence over generic best practices.

Do not modify application source code during `init`.

### 2. Detect the technology and environment

Identify the project type and relevant toolchain. For example, for Android projects inspect and report evidence for:

- JDK version
- Gradle version
- Android Gradle Plugin version
- Kotlin version
- compileSdk / targetSdk / minSdk
- Android Studio compatibility when it can be determined
- Java/Kotlin language level
- build variants/flavors
- dependency management

For other ecosystems, identify the equivalent runtime, compiler, package manager, framework, and build tool versions.

### 3. Separate facts from unknowns

Classify findings as:

- **Confirmed** — directly verified from project files or source code.
- **Inferred** — strongly indicated by project evidence but not explicitly configured.
- **Needs confirmation** — an architectural or environment decision cannot be safely determined from the repository.

Never silently invent missing versions, architectural rules, or style preferences.

### 4. Ask for confirmation when needed

If the project contains decisions that materially affect future module implementation and cannot be determined safely, stop before generating the final specification and ask the user concise confirmation questions.

Examples:

```text
I found the following unresolved project decisions:

1. JDK: project files indicate Java 17. Should new modules use Java 17?
2. Architecture: existing features use both MVVM and MVI. Which pattern should new modules use?
3. Dependency injection: Hilt is used in most modules, but one legacy module uses manual construction. Should new modules use Hilt?

Please confirm these choices before I generate project-spec.md.
```

Only ask about decisions that materially affect future implementation. Do not ask the user to confirm facts that are already unambiguously defined by the project.

If there are no unresolved decisions, proceed directly to generation.

### 5. Generate the specification

After required confirmations are obtained, generate `.j-recorder/project-spec.md`.

The document should contain, when applicable:

1. Project Overview
2. Technology Stack and Environment
3. Project Directory Structure
4. Architecture Overview
5. Module Boundaries and Responsibilities
6. Feature/Business Module Structure
7. Dependency Direction Rules
8. Naming Conventions
9. Code Style and File Organization
10. UI/Presentation Conventions
11. State Management Conventions
12. Data/Network/Repository Conventions
13. Dependency Injection Conventions
14. Error Handling and Logging
15. Resource and Configuration Conventions
16. Testing Conventions
17. Build and Dependency Management
18. Implementation Rules for New Modules
19. Confirmed Decisions
20. Known Exceptions / Legacy Areas

Keep the document concrete and implementation-oriented. Prefer examples based on the current repository rather than generic textbook advice.

### 6. Preserve uncertainty and exceptions

Do not turn an inferred pattern into a hard rule without evidence.

Use explicit labels such as:

- `Confirmed`
- `Observed pattern`
- `Recommended for new modules`
- `Needs confirmation`
- `Legacy exception`

Document important exceptions so future module generation does not accidentally copy legacy patterns as the preferred architecture.

### 7. Final behavior

After writing the file:

- Report the generated path.
- Summarize the most important confirmed architecture and environment decisions.
- Mention any assumptions or legacy exceptions.
- Tell the user that the specification is **not automatically enforced or injected into future coding tasks**.
- The user can explicitly ask an agent to read/use `.j-recorder/project-spec.md` when implementing a new module.

## Important Rules

- `init` is explicitly user-triggered only.
- Never automatically run `init` during normal coding.
- Never automatically apply `project-spec.md` to future coding tasks.
- Never modify application source code as part of `init`.
- Never expose or record passwords, API keys, access tokens, private keys, certificates, or other secrets.
- Never guess environment versions when they cannot be verified.
- Ask the user before resolving architecture or environment choices that are genuinely ambiguous.
- Existing code is the primary source of truth; generic best practices are secondary.
