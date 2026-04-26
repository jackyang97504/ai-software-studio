---
name: test-standards
description: "测试标准 — 单元测试、集成测试、E2E 测试规范"
paths:
  - "tests/**"
---

# Test Standards

测试代码必须遵循以下标准，确保测试可靠、可维护且有价值。

## 命名约定

```
test_[模块]_[场景]_[预期结果]
```

**正确:**
```python
def test_calculate_discount_member_gets_10_percent():
    ...

def test_user_authentication_with_invalid_password_raises_error():
    ...
```

**错误:**
```python
def test1():  # 缺少描述性名称
    ...

def test_bad_input():  # 不够具体
    ...
```

## AAA 模式

```python
# Arrange — 准备测试数据
# Act — 执行被测操作
# Assert — 验证预期结果

def test_calculate_total_with_discount():
    # Arrange
    items = [
        {"price": 100, "quantity": 2},
        {"price": 50, "quantity": 1}
    ]
    discount_rate = 0.1

    # Act
    total = calculate_total(items, discount_rate)

    # Assert
    expected = (200 + 50) * 0.9  # 225
    assert total == expected
```

## 测试隔离

- 每个测试独立运行
- 不依赖其他测试的状态
- 测试数据在测试内创建和清理
- 不使用共享的可变状态

```python
# 错误 — 共享状态
class TestUser:
    user = User()  # 类级别共享

    def test_login(self):
        self.user.login()

    def test_logout(self):
        # 可能依赖 test_login 的状态
        self.user.logout()

# 正确 — 每个测试独立
class TestUser:
    def test_login(self):
        user = User()  # 测试内创建
        user.login()
        assert user.is_authenticated

    def test_logout(self):
        user = User()  # 独立创建
        user.login()
        user.logout()
        assert not user.is_authenticated
```

## Mock 外部依赖

```python
from unittest.mock import Mock, patch

def test_send_notification():
    # Mock 外部服务
    with patch('notifications.email.send') as mock_send:
        mock_send.return_value = True

        result = notify_user("test@example.com", "Hello")

        assert result is True
        mock_send.assert_called_once()
```

## 集成测试

```python
# 集成测试 — 测试组件交互
# 使用真实数据库但事务回滚

import pytest

@pytest.fixture
def db_session():
    session = create_test_session()
    yield session
    session.rollback()
    session.close()

def test_create_user(db_session):
    user_repo = UserRepository(db_session)

    user = user_repo.create(email="new@example.com", name="New User")

    assert user.id is not None
    assert user.email == "new@example.com"
```

## E2E 测试

```typescript
// Playwright E2E 测试
import { test, expect } from '@playwright/test';

test('user can login and see dashboard', async ({ page }) => {
  await page.goto('/login');

  await page.fill('[name="email"]', 'test@example.com');
  await page.fill('[name="password"]', 'password123');
  await page.click('button[type="submit"]');

  await expect(page).toHaveURL('/dashboard');
  await expect(page.locator('h1')).toContainText('Welcome');
});
```
