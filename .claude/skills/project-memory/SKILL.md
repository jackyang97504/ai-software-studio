---
name: project-memory
description: "项目记忆系统 — 跨会话持久化上下文"
argument-hint: "[read|update]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash
---

# project-memory — 项目记忆系统

管理跨会话的项目记忆，保持上下文持久化。

## 记忆文件结构

```
<project>/
├── .project-memory/
│   ├── PROJECT_CONTEXT.md    # 稳定项目事实 (只读)
│   ├── CURRENT_STATE.md     # 最新工作上下文
│   ├── DECISIONS.md         # 已做出的决策记录
│   ├── KNOWN_FAILURES.md    # 已知问题和失败
│   └── session-journal/     # 会话日志
│       ├── 2026-01-15.md
│       └── 2026-01-16.md
```

## 核心文件

### PROJECT_CONTEXT.md (只读)

稳定项目事实，不会频繁变更：

```markdown
# Project Context

## 基本信息
- **项目名称**: [名称]
- **技术栈**: Python/FastAPI, React, PostgreSQL
- **团队**: 4 developers, 1 PM

## 架构
- 微服务架构
- 主要服务: api, web, worker
- 部署: AWS ECS

## 关键约束
- 必须兼容 Python 3.11+
- 必须通过 SOC 2 审计
- 每月最多 4 小时停机时间
```

### CURRENT_STATE.md (读写)

当前工作上下文，每次会话更新：

```markdown
# Current State

## 最新变更
- 2026-01-15: 完成用户认证功能
- 2026-01-14: 开始支付模块

## 进行中
- add-paddle-payments: 实现支付集成
  - 当前: 开发中
  - 阻塞: 等待 Paddle API 凭证

## 即将开始
- add-webhook-handler: 处理 Paddle webhooks
- add-subscription-ui: 订阅管理界面

## 技术债务
- [ ] 数据库连接池大小需优化
- [ ] 日志格式不一致

## 团队状态
- @alice: on vacation until 2026-01-20
- @bob: 专注于支付模块
```

### DECISIONS.md (读写)

记录重要决策：

```markdown
# Decisions

## ADR-001: 选择 Paddle 而非 Stripe (2026-01-10)
**决策**: 使用 Paddle 作为支付提供商
**理由**:
- 自动处理国际 VAT
- 开发者体验更好
**后果**: 需要重新设计订阅数据模型

## ADR-002: 使用 JWT 而非 Session (2026-01-05)
**决策**: 使用 JWT 进行认证
**理由**:
- 无状态，更易扩展
- 跨服务共享简单
```

### KNOWN_FAILURES.md (读写)

已知问题和失败：

```markdown
# Known Failures

## 间歇性问题
- [ ] 支付 webhooks 偶尔丢失 (约 1% 概率)
  - 原因: 可能是 Paddle 重试机制问题
  - 状态: 调查中

## 已确认 Bug
- [x] 登录页面在 Safari 15.1 加载慢 — 已修复 v1.2.3
```

## 工作流程

### 会话开始时

读取项目记忆：
1. 读取 `PROJECT_CONTEXT.md` — 了解项目基础
2. 读取 `CURRENT_STATE.md` — 了解最新状态
3. 读取 `DECISIONS.md` — 了解重要决策
4. 读取 `session-journal/` 最新条目 — 了解近期工作

### 会话结束时

更新项目记忆：
1. 更新 `CURRENT_STATE.md` — 记录变更和进展
2. 如有重要决策，添加到 `DECISIONS.md`
3. 如发现新问题，添加到 `KNOWN_FAILURES.md`
4. 创建 `session-journal/YYYY-MM-DD.md` — 记录本次会话

## 命令

| 命令 | 用途 |
|------|------|
| `/project-memory read` | 读取所有项目记忆 |
| `/project-memory update` | 更新项目记忆 |
| `/project-memory decision` | 记录新决策 |
| `/project-memory note` | 记录问题或笔记 |

## 示例

```
User: /project-memory read

AI:  Reading project memory...

     PROJECT CONTEXT
     ───────────────
     Project: SaaS Platform
     Stack: Python/FastAPI, React, PostgreSQL
     Team: 4 devs, 1 PM

     CURRENT STATE
     ────────────
     Latest: User auth completed
     In Progress: add-paddle-payments (blocked: waiting for API keys)
     Next: webhook handler, subscription UI

     RECENT DECISIONS
     ───────────────
     ADR-001: Paddle chosen over Stripe (2026-01-10)
     ADR-002: JWT over sessions (2026-01-05)

     KNOWN ISSUES
     ────────────
     Webhook reliability investigation in progress
```

## 输出

1. 项目上下文摘要
2. 当前状态
3. 重要决策
4. 已知问题
