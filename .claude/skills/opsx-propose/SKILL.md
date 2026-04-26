---
name: opsx-propose
description: "创建新变更并生成规划文档 — 提议 -> 规格 -> 设计 -> 任务"
argument-hint: "[change-name-or-description]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# opsx:propose — 创建变更并生成规划文档

当此技能被调用时，创建新变更并一次性生成所有规划文档。

## 工作流程

### 步骤 1: 创建变更文件夹

在 `production/changes/` 下创建变更目录：

```
production/changes/<change-name>/
├── proposal.md      # 为什么做这个变更
├── specs/           # 规格说明 (delta specs)
│   └── <domain>/
│       └── spec.md
├── design.md        # 技术设计
└── tasks.md        # 实施任务清单
```

### 步骤 2: 生成 Proposal (`proposal.md`)

```markdown
# Proposal: <变更名称>

## Intent (意图)
[一句话描述为什么要做这个变更]

## Scope (范围)
**In scope:**
- [范围 1]
- [范围 2]

**Out of scope:**
- [明确不在范围内的内容]

## Approach (方法)
[高层次的技术方法描述]

## Success Criteria (成功标准)
- [ ] [标准 1]
- [ ] [标准 2]
```

### 步骤 3: 生成 Delta Specs (`specs/<domain>/spec.md`)

使用 Given/When/Then 格式：

```markdown
# <Domain> Specification

## Purpose
[这个规格的用途说明]

## Requirements

### Requirement: <需求名称>
[系统应该做什么，使用 SHALL/MUST/SHOULD 关键词]

#### Scenario: <场景名称>
- GIVEN [前置条件]
- WHEN [触发动作]
- THEN [预期结果]
- AND [额外结果，如适用]

#### Scenario: <边界场景>
- GIVEN [边界条件]
- WHEN [动作]
- THEN [结果]
```

**RFC 2119 关键词:**
- **SHALL/MUST** — 绝对要求
- **SHOULD** — 推荐，但有例外
- **MAY** — 可选

### 步骤 4: 生成 Design (`design.md`)

```markdown
# Design: <变更名称>

## Technical Approach
[技术方法和架构决策]

## Architecture Decisions

### Decision: <决策标题>
[决策内容]
**理由:**
- [理由 1]
- [理由 2]

## Data Model

```sql
-- [数据库变更，如适用]
```

## API Design

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/resource | 获取列表 |

## Security Considerations
- [安全考虑 1]
- [安全考虑 2]
```

### 步骤 5: 生成 Tasks (`tasks.md`)

```markdown
# Tasks: <变更名称>

## Implementation Checklist

- [ ] 1.1 [任务描述]
- [ ] 1.2 [任务描述]
- [ ] 2.1 [任务描述]
- [ ] 2.2 [任务描述]

## Dependencies
- [依赖 1]
- [依赖 2]

## Verification
Run `/opsx:verify <change-name>` after implementation.
```

## 示例

```
User: /opsx:propose add-user-authentication

AI:  Created production/changes/add-user-authentication/
     ✓ proposal.md
     ✓ specs/auth/spec.md
     ✓ design.md
     ✓ tasks.md
     Ready for implementation. Run /opsx:apply.
```

## 输出

1. 变更文件夹结构
2. 所有规划文档
3. 下一步提示 (运行 `/opsx:apply`)
