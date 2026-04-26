---
name: design
description: "系统设计工作流 — 创建架构设计、API 契约和技术规格"
argument-hint: "[system-name]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, AskUserQuestion
---

# Design — 系统设计工作流

当此技能被调用时，帮助用户完成系统设计并生成技术规格。

## 工作流程

### 步骤 1: 理解系统上下文

1. 读取相关 PRD (`docs/requirements/[feature]-prd.md`)
2. 了解现有架构 (`docs/design/architecture.md`)
3. 询问技术约束和偏好

### 步骤 2: 创建架构设计文档

使用以下模板创建 `docs/design/[system-name]-design.md`：

```markdown
# [系统名称] 技术设计

## 1. 概述

[系统目标和范围]

## 2. 架构图

[系统架构图，使用 Mermaid 或 ASCII]

## 3. 组件设计

### 3.1 [组件 A]

**职责**: [组件职责]

**接口**:
```yaml
POST /api/v1/resource
Request:
  body: { ... }
Response:
  200: { ... }
  400: { error: ... }
```

**数据模型**:
```sql
CREATE TABLE resource (
  id UUID PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 3.2 [组件 B]

## 4. API 设计

### 4.1 REST API

| 方法 | 端点 | 描述 |
|------|------|------|
| GET | /api/v1/resource | 获取列表 |
| POST | /api/v1/resource | 创建资源 |

### 4.2 消息队列

**主题**: [topic-name]

**消息格式**:
```json
{
  "event": "resource.created",
  "timestamp": "2024-01-01T00:00:00Z",
  "data": { ... }
}
```

## 5. 数据流

```
[用户] → [API Gateway] → [Service A] → [Database]
                                      ↓
                                 [Message Queue]
                                      ↓
                                 [Service B]
```

## 6. 安全设计

- 认证: JWT Token
- 授权: RBAC
- 加密: TLS 1.3, AES-256

## 7. 部署架构

- 环境: dev, staging, production
- 容器化: Docker, Kubernetes
- 副本数: 3 (高可用)

## 8. 监控和告警

- SLO: 可用性 99.9%
- SLI: 延迟 P99 < 200ms
- 告警: 错误率 > 1%

## 9. 技术选型

| 组件 | 选型 | 理由 |
|------|------|------|
| 语言 | Python 3.11 | 团队熟悉 |
| 框架 | FastAPI | 高性能、自动文档 |
| 数据库 | PostgreSQL 15 | 可靠、JSON 支持 |
```

### 步骤 3: ADR (架构决策记录)

如有必要，创建 ADR 记录重要决策：

```markdown
# ADR-001: 使用 PostgreSQL 而非 MongoDB

## 状态
已接受

## 背景
需要为 [系统] 选择主数据库。

## 决策
使用 PostgreSQL 15 作为主数据库。

## 理由
- ACID 事务支持
- 团队熟悉 PostgreSQL
- JSON 支持满足灵活 schema 需求

## 后果
- 需要管理数据库迁移
- 水平扩展需要额外工作
```

## 输出

1. 技术设计文档路径
2. API 契约文档
3. ADR (如有必要)
4. 下一步建议
