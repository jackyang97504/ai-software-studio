---
name: kanban-board
description: "看板管理 — 管理工作流程、可视化任务状态"
argument-hint: "[add|move|list]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# Kanban Board — 看板管理工作流

当此技能被调用时，帮助用户管理工作流程和可视化任务状态。

## 命令

- `/kanban-board add` — 添加新任务
- `/kanban-board move` — 移动任务
- `/kanban-board list` — 查看看板

## 看板结构

```
待办 (Backlog) → 就绪 (Ready) → 进行中 (In Progress) → 审查中 (Review) → 完成 (Done)
```

## 添加任务 (`/kanban-board add`)

### 步骤 1: 收集任务信息

询问：
- 任务描述
- 所属功能/Epic
- 预估时间
- 优先级

### 步骤 2: 创建任务

```markdown
## 任务记录

### Task Card

**ID**: TASK-001
**标题**: 实现用户登录功能
**描述**: 实现邮箱密码登录，包括前端表单和后端 API
**Epic**: US-001 用户认证
**优先级**: P0
**预估时间**: 4h
**负责人**: @developer

**状态**: 📋 待办
**创建时间**: 2024-01-01
**截止时间**: 2024-01-03
```

## 移动任务 (`/kanban-board move`)

### WIP 限制

| 列 | WIP 限制 |
|---|---------|
| 待办 | 无限制 |
| 就绪 | 5 |
| 进行中 | 3 |
| 审查中 | 3 |
| 完成 | 无限制 |

### 移动检查

移动前确认：
- [ ] 任务描述完整？
- [ ] 有验收标准？
- [ ] 上一状态任务完成？
- [ ] 遵守 WIP 限制？

## 查看看板 (`/kanban-board list`)

### 看板可视化

```
📋 待办 (5)              📝 就绪 (3)           🔄 进行中 (3)          👀 审查中 (2)          ✅ 完成 (12)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[TASK-001] 用户登录    [TASK-005] 注册API   [TASK-003] 登录UI   [TASK-007] 登录测试  [TASK-002] 数据库Schema
[TASK-002] 密码重置    [TASK-006] 注册UI    [TASK-004] 注册API  [TASK-008] 性能测试  [TASK-004] 用户注册
[TASK-003] 邮箱验证    [TASK-007] 邮件服务                      [TASK-006] 文档
[TASK-004] SSO集成
[TASK-005] 2FA
```

### 流程指标

| 指标 | 值 | 状态 |
|------|-----|------|
| 吞吐量 (本周) | 8 tasks | ✅ |
| 平均前置时间 | 2.5 days | ✅ |
| WIP 总数 | 8 | ⚠️ 接近限制 |
| 阻塞任务 | 2 | ❌ |

## 输出

1. 看板状态
2. 任务列表
3. 流程指标
4. 阻塞提醒
