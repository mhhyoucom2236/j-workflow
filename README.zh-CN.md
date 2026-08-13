# J-Workflow

> 将 AI 编程工作流程转化为可复用的 Prompt、工程规范和经验教训。
>
> **记录过程，提炼经验，复用知识，重建项目。**

[![GitHub](https://img.shields.io/badge/GitHub-Open%20Source-181717?logo=github)](https://github.com/mhhyoucom2236/j-workflow)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-blue)](skills/j-workflow/SKILL.md)

[English](README.md) | **中文**

## ✨ 项目简介

**J-Workflow** 是一个面向 AI 辅助软件开发的、命令式的工作流知识系统。

AI Coding Agent 可以快速编写代码，但一次会话结束后，大量有价值的开发知识通常会随着对话一起消失，例如需求、架构决策、失败尝试、调试经验、用户修改意见以及最终解决方案。

J-Workflow 的目标，就是把这些开发过程保存成**持久、可恢复、可复用的 Workflow**。

它采用显式命令激活，不会自动干扰普通 Coding。

```text
AI Coding Agent
      │
      ▼
/j-workflow:<subcommand>
      │
 ┌────┼──────────────┐
 ▼    ▼       ▼      ▼
记录  查看    Prompt  规范
 │             │       │
 ▼             ▼       ├── 工程规范
Workflow     Prompt    └── 经验教训
```

## 🚀 命令式 Skill

J-Workflow 使用类似 OpenSpec 等命令式 AI 开发工具的命名空间方式，通过 `/j-workflow:<subcommand>` 显式调用具体功能。

| 命令 | 功能 |
|---|---|
| `/j-workflow:recorder <name>` | 创建或恢复 Workflow，并记录当前开发 Session |
| `/j-workflow:show` | 查看已记录的 Workflow |
| `/j-workflow:prompt <name>` | 根据 Workflow 历史生成完整的项目重建 Prompt |
| `/j-workflow:standard [name]` | 从 Workflow 中提取工程规范和经验教训 |
| `/j-workflow:stop` | 暂停当前 Coding Context 的行为记录，但不会删除历史 |

命名空间的意义是：`j-workflow` 表示整个 Skill 家族，后面的子命令表示具体操作。

J-Workflow **不会自动激活**。只有用户显式调用 `/j-workflow:*` 时，相关 Workflow 功能才会介入。

> **兼容性说明：** `/j-workflow:*` 的具体补全和注册方式取决于宿主 AI Coding Agent。J-Workflow 定义命令命名空间和行为，宿主 Agent 负责提供实际的 Slash Command 入口。

## 🔄 持久化 Workflow

一个 **Workflow** 表示一个长期的软件开发项目。一个 Workflow 可以包含多个 **Session**，每个 Session 记录有意义的开发事件。

### 创建 Workflow

```text
/j-workflow:recorder my-project
```

第一次执行会创建：

```text
my-project
└── Session 001
```

### 恢复 Workflow

以后继续开发时再次执行：

```text
/j-workflow:recorder my-project
```

J-Workflow 会发现已有 Workflow，恢复最近状态，并创建新的 Session：

```text
my-project
├── Session 001
├── Session 002
└── Session 003
```

历史 Session 会被保留，Workflow 历史采用追加式设计。

## ⏹️ 停止记录

当某些时候不希望记录当前开发行为，可以执行：

```text
/j-workflow:stop
```

该命令只停止当前 Coding Context 的记录行为：

- 不删除历史记录
- 不修改历史 Session
- 不完成 Workflow
- 不归档 Workflow
- 不删除已有 Event

恢复记录时再次执行：

```text
/j-workflow:recorder my-project
```

> `stop` 的含义是**停止记录**，不是**结束 Workflow**。

## 🧠 记录什么？

J-Workflow 重点记录能够帮助另一个 AI Agent 理解**项目是如何以及为什么这样构建的**信息。

典型内容包括：

- 用户需求和修改意见
- 项目约束
- 架构和实现决策
- 创建、修改、删除的文件
- 有意义的命令和工具执行结果
- 依赖和配置
- 测试与验证结果
- 失败的方案
- 错误原因
- 修复方案和临时解决方案
- 重要假设
- 可复用的经验

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
完整项目生成 Prompt
```

执行：

```text
/j-workflow:prompt <name>
```

它不是简单总结最近一次对话，而是综合整个 Workflow 的历史。

生成的 Prompt 可以包含：

- 项目目标
- 功能和非功能需求
- 技术栈及版本约束
- 架构和目录结构
- API、接口和数据模型
- 配置
- 按依赖关系排序的实现步骤
- 测试和验证要求
- 错误处理和边界条件
- 安全约束
- 工程规范
- 已知失败模式
- 验收标准

## 📚 工程规范与经验教训

执行：

```text
/j-workflow:standard [name]
```

可以针对指定 Workflow，或者所有 Workflow 提取可复用的工程知识。

### Engineering Standards

提取稳定、可执行的工程规则，例如：

- 架构边界
- 命名规范
- 测试规范
- 依赖管理
- API 设计
- 日志规范
- 错误处理
- 安全规范
- 项目结构
- 构建和部署规范

### Lessons Learned

经验来自真实开发问题：

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

一次性的临时解决方案不会在没有足够证据的情况下被直接提升为通用工程规范。

## 📁 项目结构

```text
j-workflow/
├── skills/
│   └── j-workflow/
│       ├── SKILL.md
│       ├── recorder/
│       │   └── SKILL.md
│       ├── show/
│       │   └── SKILL.md
│       ├── prompt/
│       │   └── SKILL.md
│       ├── standard/
│       │   └── SKILL.md
│       └── stop/
│           └── SKILL.md
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

Workflow 历史是**唯一事实来源（Source of Truth）**，Prompt、Standards 和 Lessons 都属于派生结果。

## 🔧 安装

J-Workflow 以 AI Agent Skill 的形式提供。

克隆项目：

```bash
git clone https://github.com/mhhyoucom2236/j-workflow.git
cd j-workflow
```

根据你的 Coding Agent 所支持的方式加载：

```text
skills/j-workflow/SKILL.md
```

如果宿主 Agent 支持命名空间 Slash Command，可以将命令映射到对应的子 Skill：

```text
/j-workflow:recorder  → skills/j-workflow/recorder/SKILL.md
/j-workflow:show      → skills/j-workflow/show/SKILL.md
/j-workflow:prompt    → skills/j-workflow/prompt/SKILL.md
/j-workflow:standard  → skills/j-workflow/standard/SKILL.md
/j-workflow:stop      → skills/j-workflow/stop/SKILL.md
```

## 💡 快速开始

### 1. 开始记录

```text
/j-workflow:recorder android-app
```

Agent 会创建或恢复 `android-app` Workflow，并开始记录当前 Coding Context 中有意义的开发事件。

### 2. 正常开发

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

Recorder 关注的是有价值的开发知识，而不是记录每一个无意义的命令或文件操作。

### 3. 暂停记录

```text
/j-workflow:stop
```

Agent 仍然可以正常 Coding，但 J-Workflow 不再追加开发事件。

### 4. 恢复记录

```text
/j-workflow:recorder android-app
```

恢复已有 Workflow，并追加新的 Session。

### 5. 查看 Workflow

```text
/j-workflow:show
```

### 6. 生成项目 Prompt

```text
/j-workflow:prompt android-app
```

### 7. 提取工程知识

```text
/j-workflow:standard android-app
```

## 🎯 设计原则

1. **显式激活** — 使用命名空间命令调用，而不是偷偷改变普通 Agent 行为。
2. **显式控制记录** — 可以使用 `/j-workflow:stop` 暂停记录，并通过 Recorder 恢复。
3. **追加式历史** — 历史 Session 和 Event 始终保留。
4. **源数据与派生数据分离** — Workflow 历史是权威数据，生成文件都是可重新构建的派生结果。
5. **事实优先** — 不确定的信息保持未知，或者明确标记为推断。
6. **失败也是知识** — 失败方案、根本原因和修复过程都是有价值的开发证据。
7. **保护敏感信息** — 密码、Key、Token、私钥等绝不能进入 Workflow 历史。

## 🎯 项目目标

J-Workflow 希望成为 AI Coding Agent 的可复用知识层。

长期目标包括：

- 支持主流 AI Coding Agent
- 原生 Skill 集成
- 更好的 Prompt 重建
- 跨项目工程知识
- 可复用 Prompt 库
- Workflow 搜索与索引
- 自动传播 Lessons 和 Standards
- Agent 专属集成

## 🤝 参与贡献

欢迎贡献代码、Issue 和 Pull Request。

如果你对 Workflow 记录、Prompt 生成、知识提取或 AI Agent 兼容性有想法，欢迎参与项目。

请保持项目的核心原则：确定性行为、追加式源历史、基于证据的知识、可复现性以及敏感信息保护。

## 📄 License

项目 License 信息将在项目进一步成熟后补充。

## ⭐ 为什么需要 J-Workflow？

AI Coding Agent 正变得越来越强。下一个问题不只是：

> **如何生成代码？**

更重要的是：

> **如何保存并复用生成代码过程中产生的知识？**

J-Workflow 将 AI 辅助开发历史视为一种可以持续积累和复用的工程资产。

> **不要让一次成功的 AI Coding 会话在结束后消失。记录它、学习它，并让它可以被再次使用。**

## 🔗 Links

- Repository: https://github.com/mhhyoucom2236/j-workflow
- [English README](README.md)
- [J-Workflow Skill](skills/j-workflow/SKILL.md)
- [Recorder Skill](skills/j-workflow/recorder/SKILL.md)
- [Stop Skill](skills/j-workflow/stop/SKILL.md)
