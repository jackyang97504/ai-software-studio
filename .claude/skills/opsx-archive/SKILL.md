---
name: opsx-archive
description: "归档完成的变更 — 合并 specs 并存档"
argument-hint: "[change-name]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# opsx:archive — 归档变更

当此技能被调用时，完成变更并将其归档到存档区。

## 工作流程

### 步骤 1: 检查变更状态

1. 验证所有 artifacts 存在:
   - proposal.md ✓
   - specs/ ✓
   - design.md ✓
   - tasks.md ✓ (所有任务完成)

2. 检查是否可以归档

### 步骤 2: 同步 Delta Specs (可选)

如果需要，将 delta specs 合并到主 specs：

```bash
production/changes/<change-name>/specs/ → production/specs/<domain>/
```

### 步骤 3: 移动到 Archive

将变更文件夹移动到 archive：

```
production/changes/<change-name>/
  ↓
production/changes/archive/YYYY-MM-DD-<change-name>/
```

### 步骤 4: 更新 Project Specs

更新主规格文档以反映变更。

## Archive 目录结构

```
production/changes/archive/
├── 2026-01-15-add-user-authentication/
│   ├── proposal.md
│   ├── specs/
│   ├── design.md
│   └── tasks.md
├── 2026-01-16-fix-login-redirect/
│   └── ...
```

## 示例

```
User: /opsx:archive add-user-authentication

AI:  Archiving add-user-authentication...

     Artifact status:
     ✓ proposal.md exists
     ✓ specs/auth/ exists
     ✓ design.md exists
     ✓ tasks.md exists (12/12 tasks complete)

     Syncing delta specs to production/specs/auth/...
     ✓ Merged specs/auth/spec.md

     Moving to archive...
     ✓ Moved to production/changes/archive/2026-01-15-add-user-authentication/

     Archive complete!
     Specs updated. Ready for the next feature.
```

## 批量归档

如果有多个完成的变更，使用 `/opsx:bulk-archive`。

## 输出

1. Artifact 状态确认
2. Specs 同步结果
3. Archive 位置
4. 清理确认
