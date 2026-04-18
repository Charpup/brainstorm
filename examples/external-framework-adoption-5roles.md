# Example: 外部框架借鉴决策（5 角色对抗模板 + R2 条件让步）

> 脱敏自 TriaDev Round-3 Session（2026-04-18）—— 议题是"TriaDev 工作流是否借鉴 Superpowers (158k stars) 进行改造"。展示 5 角色模板 + R1→R2 迭代 + 条件让步收敛的完整闭环。

## 议题

用户有一套已投入使用的工作流 skill（称为 X），考虑是否借鉴热门外部框架 Y（158k stars）进行改造。已做过深度对比分析：7 维度矩阵 + 6 互补点 I1-I6 + 5 冲突点 C1-C5 + 7 借鉴项按 P0/P1/P2 分级。初步倾向"扩展，不替换"。

## 角色派发

5 角色并行（议题有"推崇 vs 守护 vs 用户代言 vs 质疑 vs 落地"五种张力）：

| 角色 | 立场核心 | 框架 |
|------|---------|------|
| **Framework Evangelist** | 推崇 Y 的设计哲学，挑战 X 的保守 | agent 体验 > 人工契约；自愈 workflow > 预定 DAG |
| **Existing-Asset Architect** | 守护 X 的独特资产（JSON 契约、量化 rubric），在借鉴诱惑前画边界 | machine-verifiable > human-approvable；中央协调 > 水平扩展 |
| **User-Scenario Guardian** | 代言真实使用者（单人 PM），审视每条改造是否真帮到他 | self-review > 社交迁就；自动化规则 > 流程仪式；决策疲劳成本 > 完备性 |
| **Anti-Cargo Challenger** | 怀疑跟风，追问每条改造是否解决已观察 drift | 证据 > 时尚；YAGNI；回滚成本 |
| **Migration Realist** | 纯落地：PR 数、文件数、schema 影响 | 边际成本 > 理论收益；兼容性矩阵 > 新功能列表 |

每个 agent prompt 都注入关键技术事实（如 Y 框架 v5.0.6 回调自己的 subagent 机制；X 框架 Round-2 刚合并 0 周无 drift 数据；8 次 PR × 0 次 worktree 使用等**反事实统计**）。

## R1 输出摘要（5 角色）

### 共识（4 条，所有角色同意）
1. **不借鉴 Y 的 fresh subagent per task**（Y 自己 v5.0.6 也在回调）
2. **分 Round 渐进，不一次性改造**（回滚 O(1) > O(n)）
3. **Y 独有的"Git Worktree 隔离"值得借鉴**（X 框架完全没有）
4. **X 框架的 value-first-gate 已经做了 Y 的 HARD-GATE 要做的事**（避免重复）

### 核心分歧（3 项）
- **分歧 1**：是否加 Y 的 HARD-GATE → 3 vs 1 否决（Architect/Guardian/Challenger vs Evangelist）
- **分歧 2**：是否加 Y 的 two-stage review I2 → 未收敛（Evangelist 强推 vs Architect 举出"破坏 .tdd-state.json schema"技术硬伤）
- **分歧 3**：现在做 vs 等 2 周 drift 数据 → 意见分裂

## R2 聚焦迭代（3 角色，非全员）

编排者判断：分歧 1 已清晰裁决；分歧 3 可由 Architect 折中方案"做低风险子集"覆盖；**仅分歧 2（I2 两阶段 review）需再辩一轮**。

R2 fire 3 角色（Evangelist / Architect / Guardian），聚焦 I2：
- prompt 明确"R1 已定结论列表"（继承），"本轮只辩 I2"（聚焦）
- 每个 agent 被要求"提出 R1 未覆盖的全新维度"

### R2 关键进展（条件让步收敛）

**Evangelist 提出 lean 实现路径**：
> I2 不必破坏 .tdd-state.json schema。可做"0.3 版 I2"——在现有 Phase 4 REFACTOR 完成后加一段 inline checklist（纯 SKILL.md 规范），零 schema migration。

**Architect 从"绝对反对"转"条件接受"**（这是 R2 最有价值的收敛）：
> 接受 Evangelist 的 lean 实现路径**仅限于** Round-3 之后，且满足：① P0.1 verification skill 已运行 ≥1 个项目周期产生 drift 数据；② R3 Devil's Advocate 真实发现率 < 65%（证明有漏洞需补）；③ 若两条都满足，改作 P1.2（优先级降）而非 P0.2。

**Guardian 提出 "agent-facing vs user-facing" 新维度**：
> I2 的 inline checklist 是给 subagent 看的，不是给单人 PM 看的。solo 场景下 agent 自审仍是 rubber-stamp 温床——除非 checklist 结果在 **用户可见的 task summary** 输出。这是 R1 所有角色都漏了的 angle。

## 综合裁决

**决策**：扩展，不替换，**精准借鉴**
- **做**（Round-3）：P0.1 verification skill（4 角色共识）+ P1 git worktree（2 角色强推 + 2 角色背书）
- **冻结到 Round-4（条件触发）**：P0.2 lean I2，**4 个 all-of 前置条件**——R3 真实发现率 <65% + 用户跑过完整 cycle + lean 实现 + 用户可见 block 路径
- **明确不借鉴**：HARD-GATE（R1 3 vs 1 否决）/ fresh subagent per task（5 角色共识 + Y 自身 v5.0.6 回调）

**工作量**：~7 PR × ~9 文件 × 零 schema bump × O(1) 回滚

## 这个模式的复利价值

本案例完整演示了：

### 1. 5 角色模板对"借鉴决策"的普适性
Evangelist（推）+ Architect（守）+ Guardian（用户代言）+ Challenger（质）+ Realist（落地）——每个立场对应一种真实成本/担忧，分歧本质是"不同成本的权衡"。任意"是否借鉴/采纳/替换 X 框架"议题都适用。

### 2. 条件让步作为理想收敛信号
R2 Architect 的"绝对反对 → 条件接受（指标 <65% 触发）"是教科书式收敛：
- 留了**数据门**（未来可观测验证）
- 给了**明确行动路径**（观察 R3 发现率）
- 允许**后续迭代**（R3/R4 基于真实数据再决）

比"少数服从多数"健康得多——因为没有哪一方"输了"，只是达成了"按条件分阶段实施"的共识。

### 3. 反事实统计的杀伤力
议题注入"过去 8 次 PR × 0 次 Y 框架 REQUIRED 机制使用" 这种 absence-of-signal 事实，让 Anti-Cargo Challenger 用"想象的风险 vs 真实 0 次发生"做最硬的反驳——比正面论证更无懈可击。

### 4. R2 聚焦子集而非全员
R1 5 角色，R2 仅 3 角色参战。省的不是成本，是"避免让已达成共识的角色再唱和"的信号稀释。

## 反模式：选项锁定（如本 session 后续议题遇到）

在相邻的议题（如"是否把 using-git-worktrees 耦合到主流程 (a)/(b)/(c)/(d)"）中，2 个角色绕过议题给的 a/b/c/d 编号自创了选项，导致综合时需要手工 map。**编排者应在 subagent prompt 明确要求：如果议题提供编号选项，推荐必须 map 回原编号或显式声明对应关系**（此要求已编入 SKILL.md "额外要求"第 4 条）。
