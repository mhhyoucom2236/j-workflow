---
name: j-workflow-init
description: Inspect a user-specified project, directory, module, or engineering area to establish its architecture, engineering conventions, code style, and environment baseline. Use only when the user explicitly invokes /j-workflow:init. Optional scope and additional instructions may be provided after the command. Ask the user to confirm unresolved project decisions before generating the project specification.
---

# J-Workflow Init

Use this skill only when the user explicitly invokes:

```text
/j-workflow:init
```

The command also accepts an optional **scope** and **additional instructions**:

```text
/j-workflow:init <scope> [additional instructions]
```

Examples:

```text
/j-workflow:init
/j-workflow:init app
/j-workflow:init modules/payment
/j-workflow:init feature/user "重点识别这个目录的架构、代码风格以及新增业务模块应该遵循的规范"
/j-workflow:init androidApp "只分析 Android 工程，不分析后端和脚本；重点检查 JDK、Gradle、AGP、Kotlin 版本"
```

## Scope and Additional Instructions

When the user provides text after `/j-workflow:init`, interpret it as **user-provided analysis guidance**, not as a new requirement to modify the project.

### Scope

If the first argument identifies a valid directory, module, package, feature, or other project area, treat it as the **primary analysis scope**.

For example:

```text
/j-workflow:init modules/payment
```

means:

> Analyze `modules/payment` as the primary engineering area. Inspect related parent/root configuration and dependencies when necessary to correctly understand this area, but do not treat unrelated modules as the primary subject.

The scope can be:

- A directory path
- A module name
- A package or source area
- A feature/business domain
- A clearly identifiable project component

If the supplied scope does not exist or is ambiguous, do **not** silently choose another directory. Ask the user to clarify the scope.

### Additional instructions

Text after the scope may contain additional analysis guidance, such as:

```text
/j-workflow:init modules/payment "重点分析支付模块，识别它应该如何新增业务功能"
```

The additional instructions can tell the Agent:

- What part of the project deserves more attention
- Which architecture or coding conventions to identify
- Which files/modules should be compared
- Which environment/toolchain versions should be checked
- What kind of future module the specification should help implement
- What should explicitly be excluded from the analysis

Additional instructions **refine the analysis**. They must not override the core safety and behavior rules of this Skill.

For example, if the user says:

```text
/j-workflow:init app "只分析 app 目录，并参考现有 user 和 order 模块，总结以后新增业务模块应该遵循的架构"
```

the Agent should:

1. Treat `app` as the primary scope.
2. Inspect `user` and `order` as representative reference modules when they are relevant.
3. Compare their implementation patterns.
4. Distinguish established patterns from one-off or legacy implementations.
5. Generate a specification focused on the requested engineering area.

## Output Scope

Generate or update:

```text
.j-recorder/project-spec.md
```

The generated specification should clearly state the analyzed scope, for example:

```markdown
## Analysis Scope

- Primary scope: `modules/payment`
- Reference areas: `modules/user`, `modules/order`
- Additional user instructions: Focus on architecture and conventions for new business modules.
```

If the scope represents only one module or engineering area, do not misleadingly describe the resulting document as if every part of the repository had been equally analyzed.

## Purpose

`init` creates a reusable project specification from the **actual current project**. It is intended to help future AI coding work produce modules that match the existing project's architecture, engineering conventions, environment, and code style.

The generated specification is a reference artifact for the user. **Do not automatically apply it to unrelated coding tasks, inject it into every future prompt, or tell the agent to follow it unless the user explicitly asks to use the specification.**

## Core Procedure

### 1. Resolve the analysis scope

Before inspecting the project, determine whether the user supplied a scope.

- No scope → analyze the current project as a whole.
- Valid scope → analyze that area as the primary scope.
- Ambiguous or nonexistent scope → ask the user before proceeding.

Inspect related root/module configuration when needed to understand the scoped area, but keep the requested scope as the center of the analysis.

### 2. Inspect before asking

Inspect the current project and collect evidence from:

- Top-level directory structure relevant to the requested scope
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

### 3. Detect the technology and environment

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

If the user supplied additional instructions about environment standards, give those checks priority within the requested scope.

### 4. Separate facts from unknowns

Classify findings as:

- **Confirmed** — directly verified from project files or source code.
- **Inferred** — strongly indicated by project evidence but not explicitly configured.
- **Needs confirmation** — an architectural or environment decision cannot be safely determined from the repository.

Never silently invent missing versions, architectural rules, or style preferences.

### 5. Ask for confirmation when needed

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

### 6. Generate the specification

After required confirmations are obtained, generate `.j-recorder/project-spec.md`.

The document should contain, when applicable:

1. Analysis Scope
2. Project Overview
3. Technology Stack and Environment
4. Project Directory Structure
5. Architecture Overview
6. Module Boundaries and Responsibilities
7. Feature/Business Module Structure
8. Dependency Direction Rules
9. Naming Conventions
10. Code Style and File Organization
11. UI/Presentation Conventions
12. State Management Conventions
13. Data/Network/Repository Conventions
14. Dependency Injection Conventions
15. Error Handling and Logging
16. Resource and Configuration Conventions
17. Testing Conventions
18. Build and Dependency Management
19. Implementation Rules for New Modules
20. Confirmed Decisions
21. Known Exceptions / Legacy Areas

Keep the document concrete and implementation-oriented. Prefer examples based on the current repository rather than generic textbook advice.

### 7. Preserve uncertainty and exceptions

Do not turn an inferred pattern into a hard rule without evidence.

Use explicit labels such as:

- `Confirmed`
- `Observed pattern`
- `Recommended for new modules`
- `Needs confirmation`
- `Legacy exception`

Document important exceptions so future module generation does not accidentally copy legacy patterns as the preferred architecture.

### 8. Final behavior

After writing the file:

- Report the generated path.
- Report the actual analysis scope.
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
- A user-provided scope must be respected; do not silently broaden or replace it.
- User-provided additional instructions refine analysis, but do not override the core behavior and safety rules of this Skill.
