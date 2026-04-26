---
name: deploy
description: "部署工作流 — 配置 CI/CD、执行部署、回滚"
argument-hint: "[plan|execute|rollback]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# Deploy — 部署工作流

当此技能被调用时，帮助用户配置 CI/CD 或执行部署。

## 命令

- `/deploy plan` — 规划部署
- `/deploy execute` — 执行部署
- `/deploy rollback` — 回滚部署

## 部署规划 (`/deploy plan`)

### 步骤 1: 收集信息

1. 读取当前版本和环境
2. 了解目标环境配置
3. 确定部署策略

### 步骤 2: 部署检查清单

```markdown
## 部署前检查

### 代码检查
- [ ] 所有 PR 已合并到 main
- [ ] 代码审查已批准
- [ ] 单元测试通过
- [ ] 集成测试通过
- [ ] 安全扫描通过

### 环境检查
- [ ] 数据库迁移准备就绪
- [ ] 配置变更准备就绪
- [ ] 依赖服务可用
- [ ] 监控告警已配置

### 沟通检查
- [ ] 团队已通知
- [ ] On-call 已确认
- [ ] 备用方案已准备

### 回滚计划
- [ ] 回滚步骤已记录
- [ ] 回滚脚本已测试
- [ ] 数据库备份已完成
```

### 部署策略选择

| 策略 | 适用场景 | 风险 |
|------|---------|------|
| Blue-Green | 有状态服务 | 高 |
| Canary | 高风险变更 | 中 |
| Rolling | 无状态服务 | 低 |

## 部署执行 (`/deploy execute`)

### CI/CD 流水线示例

```yaml
# GitHub Actions
name: Deploy

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run tests
        run: npm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Build Docker image
        run: |
          docker build -t app:${{ github.sha }} .
          docker push registry/app:${{ github.sha }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - name: Deploy to staging
        run: kubectl apply -f k8s/staging/

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy to production
        run: kubectl apply -f k8s/production/
```

## 回滚执行 (`/deploy rollback`)

### 回滚步骤

1. **确认回滚原因**
2. **执行数据库回滚** (如需要)
3. **部署上一版本**
4. **验证服务恢复**
5. **通知团队**

### 回滚命令

```bash
# Kubernetes 回滚
kubectl rollout undo deployment/app

# Docker Compose 回滚
docker-compose down
docker-compose -f docker-compose.backup.yml up -d

# 数据库回滚
psql -h db -U app < rollback.sql
```

## 输出

1. 部署检查清单
2. 部署策略建议
3. 回滚计划
4. 部署后验证步骤
