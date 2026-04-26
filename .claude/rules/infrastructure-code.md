---
name: infrastructure-code
description: "基础设施代码规则 — IaC、Terraform、Kubernetes 标准"
paths:
  - "src/infrastructure/**"
  - "ops/**"
---

# Infrastructure Code Rules

基础设施代码必须遵循以下标准，确保基础设施可重复、安全且合规。

## Terraform

```hcl
# 模块化设计
# 环境分离

module "ecs_cluster" {
  source  = "./modules/ecs"
  version = "1.0.0"

  cluster_name = var.cluster_name
  environment  = var.environment

  # 资源 tagging
  tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
    Project     = var.project_name
  }
}

# 变量验证
variable "environment" {
  type        = string
  description = "Environment name"
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}

# 输出敏感信息处理
output "db_password" {
  value     = random_password.db.result
  sensitive = true
}
```

## Kubernetes

```yaml
# 资源限制必须设置
# 健康检查必须配置
# 副本数满足可用性

apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-server
  labels:
    app: api-server
    environment: production
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: api-server
  template:
    metadata:
      labels:
        app: api-server
    spec:
      containers:
        - name: api
          image: registry/app:v1.2.3
          ports:
            - containerPort: 8080
          resources:
            limits:
              cpu: "500m"
              memory: "512Mi"
            requests:
              cpu: "200m"
              memory: "256Mi"
          livenessProbe:
            httpGet:
              path: /health
              port: 8080
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /ready
              port: 8080
            initialDelaySeconds: 5
            periodSeconds: 5
          env:
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: api-secrets
                  key: database-url
```

## 通用规则

### 安全性
- 最小权限 IAM
- 不存储明文秘密
- 网络隔离 (VPC)
- 加密存储和传输

### 幂等性
- Terraform 配置可重复执行
- 多次运行结果一致
- 破坏性操作需确认

### 审计
- 所有变更在 Git
- 变更有审批流程
- 执行有日志记录

### 成本优化
- 合理选择实例类型
- 使用预留实例
- 自动扩缩容
