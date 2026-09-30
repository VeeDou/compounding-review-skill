# Compounding Review Skill

<p align="center">
  <a href="./README.md">English</a> | <strong>简体中文</strong>
</p>

一个面向 AI Coding Agent 的任务后复盘 Skill：**给 AI 反馈与复核，给人沉淀可复用的经验。**

大多数 AI Agent 工作流都在优化一件事：**把当前任务做完**。

但当 Codex、DeepSeek Harness 或其他 Coding Agent 帮你完成一次开发、调试、研究或多 Agent 协作之后，真正有长期价值的东西往往没有留下来：

- AI 这次哪里做对了，哪里绕路了？
- 它原本预期什么，实际又发生了什么？
- 两个 Agent 为什么要协作？第二个 Agent 到底提供了什么增量价值？
- 这次工作里，有什么机制、判断或失败模式值得人真正理解？
- 下次遇到类似问题，应该更早想到什么、检查什么？

**Compounding Review Skill** 就是放在任务结束后的这一层。

## 核心思路

一次开发、调试、研究、设计或多 Agent 任务结束后，生成两个目标完全不同的产物。

### 🤖 给 AI：反馈与复核（AI REVIEW）

目标不是给 AI 再写一份“记忆”，而是让下一次执行更好。

它会复核：

- 任务是否真的完成，还是只是“看起来完成”；
- 哪些策略有证据证明有效；
- 哪里存在无效搜索、过早假设、重复工作或缺少验证；
- 原来的预期与实际 observation 有什么差异；
- 多 Agent 协作时，每次 handoff 到底增加了什么信息或验证；
- 下一轮应该 **KEEP / CHANGE / TRY / VERIFY** 什么。

### 👤 给人：经验沉淀（HUMAN COMPOUNDING）

这里不复述执行日志，而只留下未来值得带走的东西：

- **核心机制**：这次真正应该理解什么？
- **为什么**：背后的因果关系或系统模型是什么？
- **认知更新**：原来的理解哪里需要修正？
- **迁移规则**：尽量压缩成 `看到 X → 想到 Y → 检查 Z`；
- **可复用资产**：是否值得进一步变成 checklist、测试、decision rule、架构模式、自动化或 Agent 协作模式？
- **仍待验证**：哪些目前只是推断，而不是已经证明的结论？

## 一个例子

一次异步任务卡在 `PENDING` 的排查，普通总结可能只是：

> 修改了 timeout 和 recovery 相关代码，测试通过。

而 Human Compounding 更希望留下：

> **异步系统中，请求生命周期、持久化状态生命周期和恢复生命周期是不同的。看到任务卡在中间状态时，不要只盯 provider timeout；应同时检查状态迁移、事务和 stale recovery。**

进一步压缩成迁移规则：

`看到中间状态卡住 → 想到生命周期可能不一致 → 检查 timeout / state transition / stale recovery`

## 多 Agent：复盘协作本身

如果一次任务中存在 Planner、Research Agent、Coding Agent、Evaluator 等多个独立 Agent，Skill 会额外复盘：

`Agent A → 交付了什么信息/产物 → Agent B → 产生了什么增量结果`

重点不是证明“多 Agent 更高级”，而是判断：

- 角色拆分是否真的有价值；
- 第二个 Agent 是否发现了第一个 Agent 没发现的信息；
- 独立 Evaluator 是否真正改变了结果；
- 哪些 handoff 是有效的，哪些只是增加复杂度。

普通工具调用不会被当作多个 Agent。

## 防止 AI 事后编故事

Skill 不允许把“听起来合理的解释”直接当成事实。重要判断会区分：

- **OBSERVED**：执行轨迹、测试、diff、工具结果等直接支持；
- **STATED**：Agent 当时明确说过的理由；
- **INFERRED**：事后推断，必须明确标记；
- **UNKNOWN**：现有证据不足。

所以它不是让 AI 给自己的行为写一篇漂亮复盘，而是尽量建立一个**有证据约束的任务后学习闭环**。

## 项目结构

```text
.
├── SKILL.md                         # 平台无关的核心 Skill
├── references/
│   └── example-output.md            # 输出示例
└── adapters/
    ├── codex.md                     # Codex 接入说明
    └── deepseek-harness.md          # DeepSeek Harness 接入说明
```

## 使用方式

将这个目录安装或暴露为 Agent Skill，在有实质内容的任务结束后调用 `compounding-review`。

核心行为只维护在 [`SKILL.md`](SKILL.md) 中；Codex、DeepSeek Harness 等平台的触发与运行时适配放在 adapters 中，避免维护多份行为规范。

推荐的路由规则：

> 在有实质内容的编码、调试、研究、设计或多 Agent 委派任务结束时，在最终回复前使用 `compounding-review`。简单修改可以跳过或极简输出。必须把给 AI 的反馈/复核与给人的经验沉淀分开。

接入方式参见 [Codex adapter](adapters/codex.md) 和 [DeepSeek Harness adapter](adapters/deepseek-harness.md)。

## 设计原则

1. **不总结整段对话。** 只重建最小但有用的因果/决策模型。
2. **验证优先于合理叙事。** 优先相信测试、diff、工具结果、Evaluator 输出和明确陈述。
3. **多 Agent 不天然更好。** 要找出每次 handoff 的增量价值；没有就明确说没有。
4. **不强行制造经验。** 简单任务输出 `Human Compounding: none` 完全合法。
5. **推断必须标记。** 不能把事后合理解释包装成已经观察到的事实。
6. **优化未来价值，而不是笔记数量。** 一个真正可复用的规则或测试，通常比又一篇总结更有价值。

## 当前状态

**v0.1 — experimental。** 当前目标是先在真实的 Codex 和 DeepSeek Harness 任务中验证这种复盘格式，再决定是否增加自动触发、存储、评分或检索系统。

欢迎贡献真实使用案例和改进建议。

## License

MIT
