# brainstorm — 多角色并行头脑风暴 Skill

一个 Claude Code skill，将议题同时交给多个角色鲜明的 subagent 独立分析，汇聚多视角 insights，输出有立场、有行动的结构化报告。

## 快速开始

```
/brainstorm <议题>
/brainstorm --roles "角色1,角色2,角色3" <议题>
```

**示例：**
```
/brainstorm 我们的 harness 要不要在工具调用前加一个 human-in-the-loop 确认步骤？
/brainstorm --roles "品牌视角,用户视角,竞品视角" 我们的 AI 效率工具叫什么名字好？
```

## 默认角色

| 角色 | 关切焦点 |
|------|---------|
| **AI Infra Builder** | Harness 架构：记忆层、权限管道、委派模式、context 工程、hook 生命周期 |
| **Challenger** | First-principles 假设检验 + 4 类反模式识别（整合 value-first-gate 框架） |
| **Product Manager** | 用户端交付：痛点频率/上手摩擦/MVP 范围/成功验收标准 |
| **Coder** | 代码实现：技术可行性/复杂度估算/技术债/已知坑 |

默认角色面向 **harness/agentic 系统设计** 场景。如需通用角色，通过 `--roles` 参数覆盖。

## 输出格式

每次运行产出一份结构化报告，包含：
- 各角色独立分析（3-5 要点，体现该角色特有关切）
- **综合分析**：关键共识 + 核心分歧
- **建议**：明确立场（做/不做/条件性做），不骑墙
- **后续行动**：具体可执行，含时间节点

## 安装

将整个 `brainstorm/` 目录放置于 `~/.claude/skills/brainstorm/`，重启 Claude Code 即可通过 `/brainstorm` 调用。

## 目录结构

```
brainstorm/
├── SKILL.md                    # 核心编排逻辑
├── agents/                     # 默认角色 prompt
│   ├── ai-infra-builder.md
│   ├── challenger.md
│   ├── product-manager.md
│   └── coder.md
├── references/
│   └── role-boundaries.md      # 角色边界冲突分析文档
└── evals/
    └── evals.json              # 3 组评测用例
```

## 自定义角色

通过 `--roles` 参数指定任意角色名，skill 会根据名称推断立场：

```
/brainstorm --roles "CFO,CTO,用户研究员" 我们要不要自建 LLM 基础设施？
```

## 评测结果

针对 3 个议题的对照测试（with_skill vs. baseline）：

| 场景 | with_skill | baseline |
|------|-----------|---------|
| 产品功能决策 | 5/5 = 100% | 3/5 = 60% |
| 策略方向选择 | 4/4 = 100% | 3/4 = 75% |
| 自定义角色命名 | 4/4 = 100% | 4/4 = 100% |
| **平均** | **100%** | **78%** |

关键差异：with_skill 强制产生并行角色视角，baseline 倾向于"视角/维度"等非人格化框架，缺乏角色张力。

## License

MIT
