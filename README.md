# Compounding Review Skill

A post-task review skill for AI coding agents: **feedback and verification for agents, compounding insights for humans**.

Most agent workflows optimize for finishing the current task. This skill adds a small learning layer after substantive work so that the task can improve both the next agent run and the human's long-term understanding.

<p align="center">
  <strong>English</strong> | <a href="./README.zh-CN.md">简体中文</a>
</p>

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
