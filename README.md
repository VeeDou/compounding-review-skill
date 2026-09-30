# Compounding Review Skill

A post-task review skill for AI coding agents: **feedback and verification for agents, compounding insights for humans**.

Most agent workflows optimize for finishing the current task. This skill adds a small learning layer after substantive work so that the task can improve both the next agent run and the human's long-term understanding.

## 中文介绍

大多数 AI Agent 工作流都在优化一件事：**把当前任务做完**。

但当 Codex、DeepSeek Harness 或其他 Coding Agent 帮你完成一次开发、调试、研究或多 Agent 协作之后，真正有长期价值的东西往往没有留下来：

- AI 这次哪里做对了，哪里绕路了？
- 它原本预期什么，实际又发生了什么？
- 两个 Agent 为什么要协作？第二个 Agent 到底提供了什么增量价值？
- 这次工作里，有什么机制、判断或失败模式值得人真正理解？
- 下次遇到类似问题，应该更早想到什么、检查什么？

**Compounding Review Skill** 就是放在任务结束后的这一层。

它刻意把产物拆成两部分：

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
- **迁移规则**：尽量压缩成  
  `看到 X → 想到 Y → 检查 Z`
- **可复用资产**：是否值得进一步变成 checklist、测试、decision rule、架构模式、自动化或 Agent 协作模式？
- **仍待验证**：哪些目前只是推断，而不是已经证明的结论？

例如，一次异步任务卡在 `PENDING` 的排查，普通总结可能只是：

> 修改了 timeout 和 recovery 相关代码，测试通过。

而 Human Compounding 更希望留下：

> **异步系统中，请求生命周期、持久化状态生命周期和恢复生命周期是不同的。看到任务卡在中间状态时，不要只盯 provider timeout；应同时检查状态迁移、事务和 stale recovery。**

也就是：

`看到中间状态卡住 → 想到生命周期可能不一致 → 检查 timeout / state transition / stale recovery`

### 多 Agent：不仅看“谁做了什么”，还看“为什么这样协作”

如果一次任务中存在 Planner、Research Agent、Coding Agent、Evaluator 等多个独立 Agent，这个 Skill 会额外复盘：

`Agent A → 交付了什么信息/产物 → Agent B → 产生了什么增量结果`

重点不是证明“多 Agent 更高级”，而是判断：

- 角色拆分是否真的有价值；
- 第二个 Agent 是否发现了第一个 Agent 没发现的信息；
- 独立 Evaluator 是否真正改变了结果；
- 哪些 handoff 是有效的，哪些只是增加复杂度。

### 防止 AI 事后编故事

Skill 不允许把“听起来合理的解释”直接当成事实。重要判断会区分：

- **OBSERVED**：执行轨迹、测试、diff、工具结果等直接支持；
- **STATED**：Agent 当时明确说过的理由；
- **INFERRED**：事后推断，必须明确标记；
- **UNKNOWN**：现有证据不足。

所以它不是让 AI 给自己的行为写一篇漂亮复盘，而是尽量建立一个**有证据约束的任务后学习闭环**。

### 设计原则

这个项目目前刻意保持轻量：

- 不自动保存所有对话；
- 不为了“有沉淀”而强行制造经验；
- 不把普通 tool call 误认为多 Agent；
- 不要求 Vector DB、Knowledge Graph 或复杂 Memory 系统；
- 对没有长期价值的简单任务，`Human Compounding: none` 完全合法。

目标不是积累更多笔记，而是：

> **让每一次真实的 AI 协作，在完成当前任务之外，也有机会改善下一次 Agent 的行为，并增加人的长期判断力与可复用经验。**

---

## The idea

After a coding, debugging, research, design, or multi-agent task finishes, produce two deliberately different outputs:

### AI REVIEW

For the agent(s), review the work as an evidence-grounded feedback loop:

- Was the task actually completed and verified?
- What worked, and what caused avoidable loops or weak decisions?
- What did the agent expect to happen, and what actually happened?
- If multiple agents collaborated, what did each handoff add?
- What should the next run **KEEP / CHANGE / TRY / VERIFY**?

This section is **feedback and review**, not long-term memory.

### HUMAN COMPOUNDING

For the human, extract only what is worth carrying forward:

- the mechanism or distinction worth understanding;
- why it matters beyond this task;
- a corrected or newly formed mental model;
- a transferable rule such as `When you see X -> consider Y -> check Z`;
- a candidate reusable asset such as a checklist, test, decision rule, pattern, or automation;
- remaining uncertainty.

This section is **learning and compounding**, not an execution log.

## Why separate the two?

Agents and humans need different post-task artifacts. An agent benefits from concrete behavioral feedback and verification gaps. A human benefits from causal understanding, model updates, and transferable judgment. Mixing them usually produces a generic summary that serves neither audience well.

## Evidence discipline

The skill does not invent hidden reasoning. Important claims are separated into:

- **OBSERVED** — directly supported by trajectory/artifacts.
- **STATED** — rationale explicitly stated during the task.
- **INFERRED** — post-hoc interpretation; must be labeled.
- **UNKNOWN** — insufficient evidence.

For multi-agent work, ordinary tool calls do not count as additional agents. The review focuses on actual agent/subagent/reviewer identities, information handoffs, and observable incremental value.

## Repository

```text
.
├── SKILL.md                         # platform-independent core skill
├── references/
│   └── example-output.md            # example review
└── adapters/
    ├── codex.md                     # Codex integration notes
    └── deepseek-harness.md          # DeepSeek Harness integration notes
```

## Usage

Install or expose this directory as an Agent Skill in your agent environment, then invoke `compounding-review` after substantive work. The core workflow lives in `SKILL.md`; platform-specific routing belongs in the adapters/runtime rather than being duplicated into the core prompt.

A useful routing instruction is:

> At the end of substantive coding, debugging, research, design, or delegated multi-agent work, use the `compounding-review` skill before the final response. Skip or keep it minimal for trivial edits. Separate AI feedback/verification from human-compounding insights.

See [`adapters/codex.md`](adapters/codex.md) and [`adapters/deepseek-harness.md`](adapters/deepseek-harness.md) for integration notes.

## Design principles

1. **Do not summarize the transcript.** Reconstruct the smallest useful causal/decision model.
2. **Verification beats plausible narration.** Prefer tests, diffs, tool outputs, evaluator results, and explicit statements.
3. **Multi-agent is not automatically better.** Identify the incremental value of each handoff; if there is none, say so.
4. **Do not force a lesson.** `Human Compounding: none` is a valid result for trivial or low-information work.
5. **Keep inference labeled.** Never turn a plausible post-hoc explanation into an observed fact.
6. **Optimize for future usefulness, not note count.** A reusable rule or test is often more valuable than another prose summary.

## Status

**v0.1 — experimental.** The current goal is to test the review format on real Codex and DeepSeek Harness tasks before adding heavier automation, storage, scoring, or retrieval systems.

Contributions and real-world examples are welcome.

## License

MIT
