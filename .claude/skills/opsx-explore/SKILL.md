---
name: opsx-explore
description: "探索性研究 — 在确定变更前调查问题、比较方案"
argument-hint: "[topic]"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, AskUserQuestion
---

# opsx:explore — 探索性研究

当此技能被调用时，进行探索性研究以理解问题、比较方案，为后续变更做准备。

## 与 opsx:propose 的区别

| 场景 | 使用 |
|------|------|
| 需求清晰，知道要做什么 | `/opsx:propose` — 直接生成规划文档 |
| 需求不明确，需要调查 | `/opsx:explore` — 先研究，再决定 |

## 工作流程

### 步骤 1: 理解探索主题

询问用户想要探索什么：
- 问题是什么？
- 有哪些约束条件？
- 想要达到什么目标？

### 步骤 2: 调查代码库

分析现有代码：
- 当前实现是怎样的？
- 有哪些相关组件？
- 依赖关系是什么？

### 步骤 3: 比较方案

提出多个方案并分析：

```markdown
## Options Analysis

### Option 1: [方案名称]
**优点:**
- [优点 1]
- [优点 2]

**缺点:**
- [缺点 1]

**风险:**
- [风险 1]

**适用场景:** [何时适合]

### Option 2: ...
```

### 步骤 4: 提出建议

基于分析给出建议：
- "基于你的需求，我推荐 Option X，因为..."
- "这个方案的风险是..."

### 步骤 5: 决定下一步

用户确认方向后，可以：
- 运行 `/opsx:propose <change-name>` 开始正式变更
- 继续探索其他问题

## 示例

```
User: /opsx:explore

AI:  What would you like to explore?

User: We need to add payment processing. Which payment provider should we use?

AI:  Let me investigate your current system...
     [Analyzes codebase structure, existing integrations]

     I found your system uses Stripe for subscriptions. For payment
     processing, here are three main options:

     1. Stripe Payments (expand existing)
        - Pros: Already integrated, team familiar, unified dashboard
        - Cons: Higher fees at scale
        - Best for: Most teams, fastest to implement

     2. Paddle
        - Pros: Handles VAT/tax automatically, developer-friendly
        - Cons: Less control, ecosystem lock-in
        - Best for: SaaS with international customers

     3. Direct API (Stripe + PayPal)
        - Pros: Maximum flexibility, no platform fees
        - Cons: Complex, significant development time
        - Best for: Enterprise with specific needs

     Which direction interests you?

User: Let's go with Paddle since we're targeting international customers.

AI:  Good choice! Paddle handles tax compliance automatically.

     Ready when you are. Run /opsx:propose add-paddle-payments to begin.
```

## 输出

1. 问题分析
2. 方案比较
3. 建议推荐
4. 下一步建议 (运行 `/opsx:propose`)
