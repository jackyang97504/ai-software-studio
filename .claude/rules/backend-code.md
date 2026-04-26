---
name: backend-code
description: "后端代码规则 — 多语言后端开发标准"
paths:
  - "src/backend/**"
---

# Backend Code Rules

后端代码必须遵循以下标准，确保代码可维护、安全且高效。

## 多语言支持

### Python

```python
# 标准: PEP 8
# 类型提示: 必须
# Docstring: Google/NumPy style

from typing import Optional, List
import logging

logger = logging.getLogger(__name__)

def get_user(user_id: int) -> Optional[dict]:
    """Get user by ID.

    Args:
        user_id: User's unique identifier

    Returns:
        User dict or None if not found

    Raises:
        ValueError: If user_id is invalid
    """
    if user_id <= 0:
        raise ValueError("user_id must be positive")
    ...
```

### Go

```go
// 标准: gofmt
// 错误处理: 包装错误
// Context: 必须传播

func GetUser(ctx context.Context, id int) (*User, error) {
    if id <= 0 {
        return nil, fmt.Errorf("invalid user id: %d: %w", id, ErrInvalidInput)
    }
    // implementation
}
```

### TypeScript (Node.js)

```typescript
// 标准: ESLint + strict mode
// 接口: 优于类型别名
// 异步: 总是使用 async/await

interface User {
  id: number;
  email: string;
  createdAt: Date;
}

async function getUser(id: number): Promise<User> {
  if (id <= 0) {
    throw new ValidationError('Invalid user id');
  }
  // implementation
}
```

### Rust

```rust
// 标准: clippy
// 错误处理: Result 类型
// 生命周期: 明确标注

#[derive(Debug)]
pub enum Error {
    NotFound,
    InvalidInput(String),
}

pub fn get_user(id: i32) -> Result<User, Error> {
    if id <= 0 {
        return Err(Error::InvalidInput(format!("invalid id: {}", id)));
    }
    // implementation
}
```

## 通用规则

### API 设计
- RESTful 命名约定
- 正确的 HTTP 状态码
- 统一的错误响应格式

### 错误处理
- 区分可恢复/不可恢复错误
- 记录足够的日志信息
- 不泄露敏感信息

### 数据库
- 使用 ORM 或 Query Builder
- 参数化查询防注入
- 添加适当的索引

### 安全
- 禁止硬编码秘密 (使用环境变量)
- 验证所有输入
- 最小权限原则

### 测试
- 单元测试覆盖核心逻辑
- 集成测试覆盖 API
- 使用 AAA (Arrange-Act-Assert) 模式
