---
name: opsx-verify
description: "验证实现是否符合规划文档 — 完整性、正确性、一致性检查"
argument-hint: "[change-name]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# opsx:verify — 验证实现

当此技能被调用时，验证实现是否符合变更的规划文档。

## 验证三个维度

| 维度 | 验证内容 |
|------|----------|
| **Completeness (完整性)** | 所有任务完成，所有需求已实现，所有场景已覆盖 |
| **Correctness (正确性)** | 实现符合规格意图，边界情况已处理 |
| **Coherence (一致性)** | 设计决策反映在代码中，模式一致 |

## 工作流程

### 步骤 1: 检查 Completeness

- [ ] 读取 `tasks.md` — 所有任务都已完成 `[x]`
- [ ] 读取 `specs/<domain>/spec.md` — 每个需求都有对应代码
- [ ] 检查场景覆盖 — 每个 Given/When/Then 场景都已测试

### 步骤 2: 检查 Correctness

- [ ] 实现符合规格意图
- [ ] 边界情况处理正确
- [ ] 错误状态符合规格定义

### 步骤 3: 检查 Coherence

- [ ] 设计决策反映在代码结构中
- [ ] 命名约定与 design.md 一致
- [ ] 架构模式正确应用

## 报告格式

```markdown
# Verification Report: <变更名称>

## COMPLETENESS
✓ All X tasks in tasks.md are checked
✓ All requirements in specs have corresponding code
⚠ Scenario "Session timeout after inactivity" not tested

## CORRECTNESS
✓ Implementation matches spec intent
✓ Edge cases from scenarios are handled
✓ Error states match spec definitions

## COHERENCE
✓ Design decisions reflected in code structure
✓ Naming conventions consistent with design.md
⚠ Design mentions "event-driven" but implementation uses polling

## SUMMARY
─────────────────────────────────────────────
Critical issues: 0
Warnings: 2
Ready to archive: Yes (with warnings)

## Recommendations
1. Add test for session timeout scenario
2. Consider refactoring to event-driven as designed, or update design.md
```

## 示例

```
User: /opsx:verify add-user-authentication

AI:  Verifying add-user-authentication...

     COMPLETENESS
     ✓ All 12 tasks in tasks.md are checked
     ✓ All requirements in specs have corresponding code
     ⚠ Scenario "Invalid token handling" not tested

     CORRECTNESS
     ✓ Implementation matches spec intent
     ✓ Edge cases from scenarios are handled
     ✓ Error states match spec definitions

     COHERENCE
     ✓ Design decisions reflected in code structure
     ✓ Naming conventions consistent with design.md
     ✓ No implementation drift detected

     SUMMARY
     ─────────────────────────────
     Critical issues: 0
     Warnings: 1
     Ready to archive: Yes (with warnings)

     Recommendations:
     1. Add test for invalid token handling scenario
```

## 输出

1. 验证报告 (完整性、正确性、一致性)
2. 问题分类 (Critical/Warning/Suggestion)
3. 建议改进项
4. 是否可以归档的结论
