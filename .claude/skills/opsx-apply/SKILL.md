---
name: opsx-apply
description: "执行变更任务 — 逐步实现 tasks.md 中的任务"
argument-hint: "[change-name]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# opsx:apply — 执行变更任务

当此技能被调用时，逐步执行变更的任务清单。

## 工作流程

### 步骤 1: 读取 Tasks

读取 `production/changes/<change-name>/tasks.md` 并识别未完成的任务。

### 步骤 2: 逐个执行任务

按顺序执行每个任务：
1. 读取任务描述
2. 理解需求和设计
3. 编写代码
4. 更新 tasks.md 标记完成 `[x]`

### 步骤 3: 标记完成

每完成一个任务，在 tasks.md 中标记为完成：

```markdown
- [x] 1.1 Create User model
- [ ] 1.2 Add password hashing
- [ ] 1.3 Implement JWT generation
```

## 示例

```
User: /opsx:apply add-user-authentication

AI:  Implementing add-user-authentication...

     Reading tasks.md:
     - [ ] 1.1 Create User model
     - [ ] 1.2 Add password hashing
     - [ ] 1.3 Implement JWT generation
     - [ ] 2.1 Create login endpoint
     - [ ] 2.2 Add authentication middleware

     Working on 1.1: Create User model...
     ✓ Created src/backend/models/user.py

     ✓ 1.1 Complete (User model created)

     Working on 1.2: Add password hashing...
     ✓ Updated src/backend/models/user.py

     ✓ 1.2 Complete (Password hashing with bcrypt)

     ...

     All tasks complete!
     Run /opsx:verify to validate implementation.
```

## 中断恢复

如果执行被中断，下次运行 `/opsx:apply <change-name>` 会从上次停止的地方继续。

## 输出

1. 实现的代码
2. 更新的 tasks.md (标记完成的任务)
3. 下一步提示 (运行 `/opsx:verify`)
