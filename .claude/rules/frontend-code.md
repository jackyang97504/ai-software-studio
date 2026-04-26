---
name: frontend-code
description: "前端代码规则 — React/Vue/Angular 开发标准"
paths:
  - "src/frontend/**"
---

# Frontend Code Rules

前端代码必须遵循以下标准，确保代码可维护、可访问且性能良好。

## React (TypeScript)

```tsx
// 组件文件: ComponentName.tsx
// Hook 文件: useComponentName.ts
// 测试文件: ComponentName.test.tsx

interface ButtonProps {
  variant: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  type?: 'button' | 'submit' | 'reset';
}

export function Button({
  variant,
  size = 'md',
  children,
  onClick,
  disabled = false,
  type = 'button'
}: ButtonProps) {
  const handleClick = () => {
    if (!disabled && onClick) {
      onClick();
    }
  };

  return (
    <button
      type={type}
      className={`btn btn-${variant} btn-${size}`}
      onClick={handleClick}
      disabled={disabled}
      aria-disabled={disabled}
    >
      {children}
    </button>
  );
}
```

## Vue (TypeScript)

```vue
<!-- 3.0 Composition API with <script setup> -->

<script setup lang="ts">
interface Props {
  variant?: 'primary' | 'secondary';
  disabled?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'primary',
  disabled: false
});

const emit = defineEmits<{
  (e: 'click'): void;
}>();

const handleClick = () => {
  if (!props.disabled) {
    emit('click');
  }
};
</script>

<template>
  <button
    :class="['btn', `btn-${props.variant}`]"
    :disabled="props.disabled"
    @click="handleClick"
  >
    <slot />
  </button>
</template>
```

## 通用规则

### 可访问性 (WCAG 2.1 AA)
- 所有交互元素可键盘访问
- 图片有 alt 属性
- 表单有关联 label
- 颜色对比度 ≥ 4.5:1
- 焦点状态可见

### 状态管理
- 组件状态用于 UI 状态
- 全局状态用于跨组件数据
- 避免 prop drilling

### 性能
- 使用 React.memo / Vue.memo
- 懒加载路由
- 虚拟列表长列表
- 优化图片

### 安全
- 防止 XSS (输出编码)
- 防止 CSRF (Token)
- 不在 localStorage 存敏感数据

### 国际化
- 所有用户文本使用 i18n
- 日期/货币格式化
- RTL 支持
