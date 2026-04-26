---
name: sprint-plan
description: "Sprint 规划工作流 — 创建 Sprint 计划、分配任务、设置目标"
argument-hint: "[new|update|status]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, AskUserQuestion
context: |
  !ls production/sprints/ 2>/dev/null || echo "No sprints directory"
---

# Sprint Plan — Sprint 规划工作流

当此技能被调用时，帮助用户规划或更新 Sprint。

## 命令

- `/sprint-plan new` — 创建新的 Sprint 计划
- `/sprint-plan update` — 更新现有 Sprint
- `/sprint-plan status` — 查看 Sprint 状态

## 工作流程

### 新建 Sprint (`/sprint-plan new`)

#### 步骤 1: 准备

1. 读取 Product Backlog
2. 查看团队速率 (历史 Sprint 数据)
3. 确定 Sprint 目标

#### 步骤 2: 选择用户故事

从 Product Backlog 选择高优先级用户故事：

| ID | 用户故事 | 故事点 | 优先级 | 状态 |
|----|---------|-------|--------|------|
| US-001 | 登录功能 | 3 | P0 | 待规划 |
| US-002 | 用户注册 | 5 | P0 | 待规划 |

#### 步骤 3: 任务分解

将用户故事分解为任务：

```markdown
### US-001: 用户登录 (3 points)

**任务**:
- [ ] Task 1: 数据库用户表设计 (2h)
- [ ] Task 2: 登录 API 实现 (4h)
- [ ] Task 3: 前端登录表单 (3h)
- [ ] Task 4: 单元测试 (2h)
- [ ] Task 5: 集成测试 (2h)
```

#### 步骤 4: 创建 Sprint 文档

创建 `production/sprints/sprint-XXX.md`：

```markdown
# Sprint [编号] — [目标]

## Sprint 信息
- **开始日期**: YYYY-MM-DD
- **结束日期**: YYYY-MM-DD
- **持续时间**: 2 周
- **团队**: [团队成员]

## Sprint 目标
[一句描述 Sprint 要达成什么]

## 承诺的待办事项

| ID | 用户故事 | 故事点 |
|----|---------|--------|
| US-001 | 登录功能 | 3 |
| US-002 | 用户注册 | 5 |
| **总计** | | **8** |

## 团队速率
- **Sprint 容量**: 20 points
- **规划点数**: 8 points
- **缓冲**: 12 points (用于突发任务)

## 每日站会

| 日期 | 进展 | 阻碍 |
|------|------|------|
| Day 1 | | |
| Day 2 | | |

## Sprint 回顾

### 做得好的
- [ ]

### 需要改进的
- [ ]

### 下个 Sprint 行动计划
- [ ]
```

### Sprint 状态 (`/sprint-plan status`)

读取当前 Sprint 文档，显示：

```
📊 Sprint 3 状态

目标: 完成用户认证系统

进度: ████████░░░░░░░░ 40%
故事点: 8/20 完成

✅ 已完成
- US-001: 登录功能 (3)

🔄 进行中
- US-002: 用户注册 (5)

📋 待开始
- US-003: 密码重置 (3)
```

## 输出

1. Sprint 计划文档
2. 任务分解
3. Sprint 目标
4. 下一步建议
