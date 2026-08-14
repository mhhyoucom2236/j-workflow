# J-Workflow

> 将 AI 编程工作流程转化为可复用的 Prompt、项目架构规范、工程标准和经验教训。
>
> **记录过程。理解项目。复用知识。持续构建。**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/j-workflow)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-workflow/SKILL.md)

[English](README.md) | **中文**

## ✨ 项目简介

**J-Workflow** 是一个面向 AI 辅助软件开发的命令式工作流知识系统。

AI Coding Agent 可以快速编写代码，但一次会话结束后，需求、架构决策、失败尝试、调试经验、用户修改意见和最终解决方案等有价值知识往往会随着对话一起消失。

J-Workflow 将这些开发过程转化为**持久、可复用的 Workflow 知识**。它采用用户显式命令激活，不会在用户未要求时干扰正常 Coding。

## ⚡ 一键让 Agent 安装

最简单的安装方式，是把下面整段 Prompt 复制给你的 AI Coding Agent。Agent 会先检查自身环境，判断它如何管理 Agent Skills，然后使用适合当前环境的方式安装 J-Workflow，而不是假设某个特定厂商或固定目录结构。

> **复制下面整个 Prompt，直接粘贴给你的 AI Coding Agent。**

```text
请帮我在当前 AI Coding Agent 环境中安装并配置 J-Workflow。

项目地址：
https://github.com/mhhyoucom2236/j-workflow

要求：

1. 先检查当前项目以及当前 AI Coding Agent 的运行环境。
2. 判断当前 Agent 支持哪一种 Agent Skills 机制，例如 skills 目录、项目级 Skills、用户/全局 Skills 或其他官方支持的机制。
3. 从上面的 GitHub 地址获取 J-Workflow 项目。
4. 使用当前宿主 Agent 官方支持的 Skill 安装/加载方式安装 J-Workflow。不要假设某个特定厂商的安装目录。
5. 确保以下 Skill 都可用：
   - j-workflow
   - j-workflow-init
   - j-workflow-recorder
   - j-workflow-show
   - j-workflow-prompt
   - j-workflow-standard
   - j-workflow-stop
6. 安装完成前，读取根 Skill 以及必要的子命令 SKILL.md 文件。
7. 如果当前 Agent 原生支持命名空间 Slash Command，则使用宿主 Agent 支持的方式提供：
   /j-workflow:init
   /j-workflow:recorder <workflow-name>
   /j-workflow:show
   /j-workflow:prompt <workflow-name>
   /j-workflow:standard [workflow-name]
   /j-workflow:stop
8. 如果当前 Agent 不支持冒号命名空间命令，则使用它实际支持的最接近的命令或 Skill 名称，不要为了模拟不支持的语法而修改 J-Workflow 的行为。
9. 验证 Skill 文件可以被 Agent 正常读取，并确认 Skill/命令可以被发现或调用。
10. 除非当前 Agent 的 Skill 安装机制明确要求，否则不要修改我的业务项目源码或项目文件。
11. 不要记录、复制或泄露密码、API Key、Access Token、私钥或其他敏感信息。

安装完成后告诉我：
- J-Workflow 安装到了哪里。
- 当前 Agent 实际可以使用哪些命令/Skill。
- 是否原生支持 /j-workflow:*，如果不支持，实际使用的命令是什么。
- 给出一个 workflow 名称为 "my-project" 的最小使用示例。
```

该 Prompt 是 **Agent-aware** 的，会让 Agent 自己判断 Codex、Claude Code、OpenCode、Cursor 或其他环境，而不是强行使用同一个安装路径。

## 🚀 命令式 Skill

| 用户命令 | 功能 |
|---|---|
| `/j-workflow:init` | 分析当前项目并创建可复用的项目架构/规范文档 |
| `/j-workflow:recorder <name>` | 创建或恢复 Workflow，并记录当前开发 Session |
| `/j-workflow:show` | 查看和检查已记录的 Workflow |
| `/j-workflow:prompt <name>` | 根据 Workflow 历史生成完整的项目重建 Prompt |
| `/j-workflow:standard [name]` | 提取工程规范和经验教训 |
| `/j-workflow:stop` | 暂停当前 Coding Context 的记录，不删除历史 |

J-Workflow **不会自动激活**。只有用户显式调用 Workflow 操作时，相关功能才会介入正常 Coding。

## 🏗️ 项目架构初始化

`/j-workflow:init` 是一个**按需执行的项目分析命令**，与 `/j-workflow:standard` 的目标不同：

- `init` 描述**当前项目的架构和实现规范**。
- `standard` 从 Workflow 历史中提取**可复用的工程规范和经验教训**。

执行：

```text
/j-workflow:init
```

Agent 会分析项目和模块结构、架构与分层边界、模块职责与依赖、命名规范、Package/文件组织、代码风格、UI/State 管理、Repository/Data/Network 模式、依赖注入、错误处理、日志、测试、构建部署配置以及项目特定约定。

对于技术特定环境，Agent 应检查相关版本。例如 Android 项目可能需要检查 JDK、Gradle、AGP、Kotlin、`compileSdk`、`targetSdk`、`minSdk`、Java/Kotlin 语言级别、Build Variants、Product Flavors 和依赖管理方式。

### 用户确认不明确的决策

`init` **不能自行猜测并制定架构规则**。如果项目存在多种模式、配置不完整，或重要决策无法可靠判断，Agent 应在最终生成规范前向用户确认。

例如：

```text
当前项目同时存在 MVVM 和 MVI 两种实现。
新模块应该使用哪一种架构？

A. MVVM
B. MVI
C. 两者都保留，根据模块类型选择
```

JDK、Gradle、AGP、Kotlin、SDK 等环境标准无法明确确定时，也应要求用户确认。

### 生成项目规范

分析并完成必要确认后，`init` 生成：

```text
.j-recorder/project-spec.md
```

该文档记录项目当前的架构、环境、约定、编码风格、模块规则和确认后的决策。

规范应区分：

- **Confirmed** — 已直接验证或由用户明确确认
- **Observed / Inferred** — 从现有实现中强烈推断，但未明确确认
- **Needs confirmation** — 尚未解决，需要用户确认

### 重要：`init` 不会自动强制执行规范

`project-spec.md` **不是自动注入所有未来 Coding 任务的全局指令**。用户决定什么时候使用它，例如：

```text
读取 .j-recorder/project-spec.md，并按照项目现有架构和编码规范实现新的支付模块。
```

这样可以保持 J-Workflow 非侵入：`init` 负责创建项目知识，何时将知识作为 Coding 指令使用仍由用户控制。

## 🔄 持久化 Workflow

一个 **Workflow** 表示一个长期开发工作。一个 Workflow 可以包含多个 **Session**，Session 包含有意义的开发事件。

创建：

```text
/j-workflow:recorder my-project
```

第一次会话：

```text
my-project
└── Session 001
```

继续开发时再次执行相同命令，J-Workflow 会恢复最新状态并创建新的 Session：

```text
my-project
├── Session 001
├── Session 002
└── Session 003
```

历史 Session 会被保留，Workflow 历史采用追加式设计。

### 停止记录

```text
/j-workflow:stop
```

该命令只停止**当前 Coding Context** 的记录行为，不会删除、修改、完成或归档 Workflow、Session 和 Event。

恢复：

```text
/j-workflow:recorder my-project
```

`stop` 的含义是**停止记录**，不是**结束 Workflow**。

## 🧠 记录什么？

J-Workflow 重点记录能够帮助另一个 AI Agent 理解**项目如何以及为什么这样构建**的信息：

- 用户需求和修改意见
- 项目约束
- 架构和实现决策
- 创建、修改、删除的文件
- 有意义的命令和工具结果
- 依赖和配置
- 测试与验证结果
- 失败方案、错误原因、修复方案和临时解决方案
- 重要假设
- 可复用经验

密码、API Key、Access Token、私钥等敏感信息绝不能写入 Workflow。

## 🧩 从 Workflow 到 Prompt

```text
开发历史
        │
        ▼
需求 + 决策
        │
        ▼
架构 + 实现
        │
        ▼
失败 + 解决方案
        │
        ▼
规范 + 经验
        │
        ▼
完整生成 Prompt
```

`/j-workflow:prompt <name>` 综合整个 Workflow 历史，而不是简单总结最近一次对话。

## 📚 工程规范与经验教训

`/j-workflow:standard [name]` 可以针对指定 Workflow 或所有 Workflow 提取可复用工程知识。

### Engineering Standards

提取稳定、可执行的规则，例如架构边界、命名、测试、依赖管理、API 设计、日志、错误处理、安全、项目结构和构建部署实践。

### Lessons Learned

```text
问题场景
    ↓
失败方案
    ↓
根本原因
    ↓
正确方案
    ↓
预防规则
```

一次性的临时解决方案不会在没有证据证明可以泛化之前，被提升为通用工程规范。

## 📁 项目结构

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

Workflow 历史是**事实来源（Source of Truth）**。Prompt、Standards、Lessons 和项目规范都是派生或经过确认的产物。

## 🔧 安装

J-Workflow 以 AI Agent Skill 的形式提供。

### 推荐：使用一键安装 Prompt

直接复制[一键让 Agent 安装](#-一键让-agent-安装)中的 Prompt 给你的 Coding Agent。Agent 会检查宿主环境并执行适合当前环境的安装方式。

### 手动安装

```bash
git clone https://github.com/mhhyoucom2236/j-workflow.git
cd j-workflow
```

对于兼容 Agent Skills 的宿主，加载对应的 `SKILL.md`：

```text
skills/j-workflow/SKILL.md
skills/j-workflow/init/SKILL.md
skills/j-workflow/recorder/SKILL.md
skills/j-workflow/show/SKILL.md
skills/j-workflow/prompt/SKILL.md
skills/j-workflow/standard/SKILL.md
skills/j-workflow/stop/SKILL.md
```

如果宿主支持命名空间 Slash Command，将命令映射到对应子 Skill：

```text
/j-workflow:init      → skills/j-workflow/init/SKILL.md
/j-workflow:recorder  → skills/j-workflow/recorder/SKILL.md
/j-workflow:show      → skills/j-workflow/show/SKILL.md
/j-workflow:prompt    → skills/j-workflow/prompt/SKILL.md
/j-workflow:standard  → skills/j-workflow/standard/SKILL.md
/j-workflow:stop      → skills/j-workflow/stop/SKILL.md
```

不要假设宿主 Agent 会因为目录存在就自动创建冒号命名空间命令。命令注册属于宿主 Agent 的集成职责。

## 💡 快速开始

### 1. 初始化项目规范

```text
/j-workflow:init
```

回答架构或环境标准方面的确认问题，结果保存到：

```text
.j-recorder/project-spec.md
```

该规范保持被动状态，直到用户明确要求 Coding Agent 使用它。

### 2. 开始记录

```text
/j-workflow:recorder android-app
```

创建或恢复 `android-app` Workflow，并记录当前 Coding Context 中有意义的开发事件。

### 3. 正常开发

```text
需求
    ↓
架构决策
    ↓
代码修改
    ↓
构建失败
    ↓
根本原因
    ↓
修复
    ↓
测试
    ↓
用户修改意见
```

Recorder 关注有价值的开发知识，而不是每一个无意义的命令或文件操作。

### 4. 暂停记录

```text
/j-workflow:stop
```

Agent 仍可正常 Coding，但 J-Workflow 不再追加开发事件。

### 5. 恢复记录

```text
/j-workflow:recorder android-app
```

恢复已有 Workflow，并追加新的 Session。

### 6. 查看 Workflow

```text
/j-workflow:show
```

### 7. 生成项目 Prompt

```text
/j-workflow:prompt android-app
```

### 8. 提取知识

```text
/j-workflow:standard android-app
```

## 🎯 设计原则

1. **显式激活** — J-Workflow 通过用户命令调用，而不是悄悄改变普通 Agent 行为。
2. **显式项目规范** — `/j-workflow:init` 创建架构知识，但不会自动强制执行。
3. **显式记录控制** — 可以通过 `/j-workflow:stop` 暂停记录，并通过 Recorder 恢复。
4. **追加式历史** — 历史 Session 和 Event 始终保留。
5. **源数据与派生数据分离** — Workflow 历史是权威数据，生成产物可以重新构建或经过确认。
6. **事实优先于猜测** — 未知信息保持未知，或明确标记为推断。
7. **重要决策由用户确认** — 对存在歧义的架构和环境选择进行确认，而不是猜测。
8. **失败也是知识** — 失败方案、根本原因和修复过程都是有价值的开发证据。
9. **保护敏感信息** — 凭据和私钥绝不能进入 Workflow 历史或生成的项目规范。

## 🤝 参与贡献

欢迎贡献代码、Issue 和 Pull Request。请保持确定性行为、追加式源历史、基于证据的知识、可复现性、显式激活和敏感信息保护。

## 📄 License

MIT License。详见 [LICENSE](LICENSE)。

## ⭐ 为什么选择 J-Workflow？

AI Coding Agent 正变得越来越强大。下一个问题不只是**如何生成代码**，还包括**如何保存并复用生成代码过程中产生的知识**。

J-Workflow 将 AI 辅助开发历史和项目架构知识视为可复用的工程资产。

> **不要让一次成功的 AI Coding 会话在会话结束时消失。记录它，理解项目，让知识真正可以复用。**

## 🔗 相关链接

- Repository: https://github.com/mhhyoucom2236/j-workflow
- [J-Workflow Skill](skills/j-workflow/SKILL.md)
- [Init Skill](skills/j-workflow/init/SKILL.md)
- [Recorder Skill](skills/j-workflow/recorder/SKILL.md)
- [Stop Skill](skills/j-workflow/stop/SKILL.md)
- [English Documentation / English README](README.md)
