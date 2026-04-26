---
name: start
description: "新项目入门引导 — 询问当前状态并引导进入正确的工作流"
argument-hint: "[open|mvp|production]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, AskUserQuestion
---

# Start — 项目引导

当此技能被调用时，引导用户完成项目初始化流程。

## 工作流程

### 步骤 1: 了解当前状态

询问用户：
1. "你当前处于什么阶段？"
   - 毫无头绪 (0) → 建议先运行 `/brainstorm`
   - 有模糊想法 (1) → 运行需求分析流程
   - 有清晰设计 (2) → 运行技术栈配置
   - 已有代码 (3) → 运行项目分析

2. "你的项目是什么类型的？"
   - Web 应用 (SaaS)
   - 移动应用
   - API 服务
   - 内部工具
   - 开源库/框架

### 步骤 2: 配置技术栈

根据项目类型，推荐技术栈：

| 类型 | 推荐后端 | 推荐前端 |
|------|---------|---------|
| Web SaaS | Python/FastAPI, Go, Node.js | React, Vue, Next.js |
| API 服务 | Go, Rust, Java | - |
| 内部工具 | Python/Django | React, Vue |
| 开源库 | Python, Rust, TypeScript | - |

运行 `/setup-engine` 配置选定的技术栈。

### 步骤 3: 创建项目结构

根据模板创建目录结构：

```
my-project/
├── src/
│   ├── backend/          # 后端代码
│   ├── frontend/         # 前端代码
│   ├── infrastructure/   # IaC
│   └── shared/           # 共享代码
├── tests/
├── docs/
│   ├── requirements/     # PRD
│   └── design/           # 架构文档
├── ops/                  # 运维脚本
└── production/           # 冲刺计划
```

### 步骤 4: 创建初始文档

1. **项目概述** (`docs/overview.md`)
2. **技术栈文档** (`docs/tech-stack.md`)
3. **第一个 Sprint 计划** (`production/sprints/sprint-001.md`)

## 输出模板

项目初始化完成后，提供：
- 项目结构概览
- 下一步建议 (下一个 `/skill`)
- 快速开始命令
