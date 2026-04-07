# Brainstorm 默认角色边界文档

> 版本：2026-04-07 | 场景：harness/agentic 系统设计为主的工作场景

---

## 角色一览

| 角色 | 关切焦点 | 对应 agents/ 文件 |
|------|---------|-----------------|
| **AI Infra Builder** | Harness 架构：记忆层、权限管道、委派模式、context 工程、hook 生命周期 | `agents/ai-infra-builder.md` |
| **Challenger** | First-principles 假设检验 + value-first-gate 4 类反模式识别 | `agents/challenger.md` |
| **Product Manager** | 用户端交付：痛点频率/强度、上手摩擦、MVP 范围、成功验收标准 | `agents/product-manager.md` |
| **Coder** | 代码实现：技术可行性、复杂度估算、技术债、已知坑 | `agents/coder.md` |

---

## 边界冲突分析

### Challenger vs Product Manager
**潜在重叠**：两者都可能质疑"用户真的需要这个吗"。

**区分点**：
- Challenger 从**假设链**切入（first-principles）：拆解"为什么我们认为这件事有价值"的底层假设，指向 Go/No-Go 决策
- PM 从**用户旅程**切入：分析用户发现、上手、习惯养成各阶段的摩擦点，指向如何更好地交付
- 张力是正常且有益的——Challenger 质疑"应不应该做"，PM 回答"如果做该怎么做"

### AI Infra Builder vs Coder
**潜在重叠**：两者都涉及技术层面。

**区分点**：
- Builder 关注**系统层**（harness 设计模式、agent 隔离方式、权限管道架构），问的是"用什么架构模式"
- Coder 关注**代码层**（具体实现、依赖选型、代码可维护性），问的是"怎么写/用什么库"
- 简单判断：Builder 的输出可以写成架构图，Coder 的输出可以写成代码注释

### Challenger vs Coder
**潜在重叠**：两者都可能提出"成本被低估"。

**区分点**：
- Challenger 是**决策层批判**：质疑整件事是否值得做，指向方向性判断
- Coder 是**实现层评估**：评估具体怎么做的复杂度，指向工程规划
- 前者是 Go/No-Go 的输入，后者是 How-to 的输入

---

## 修改角色的注意事项

1. **保持层次分离**：4 个角色覆盖决策（Challenger）、系统设计（Builder）、产品交付（PM）、工程实现（Coder）四层，修改时避免让两个角色陷入同一层次
2. **AI Infra Builder 的通用性**：对于非 harness 议题，Builder 的切入角度是"如果用 AI harness 来实现这个方案"——这是一个强制的场景迁移，保持这个角色意味着每次都会从 agentic 视角审视议题
3. **Challenger 的框架完整性**：Challenger 整合了 value-first-gate 的反模式检测，修改时保留 4 类反模式（方案优先偏差/虚荣指标/路线图货物崇拜/未定价复杂性）和 3 个紧迫性检验时间节点（30/60/90天）
