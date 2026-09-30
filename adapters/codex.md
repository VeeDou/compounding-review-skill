# Codex adapter

## Goal

Use the core `compounding-review` skill unchanged. Codex should load it at the end of substantive work or when explicitly asked for a review.

## Recommended setup

1. Install the skill directory where the Codex environment discovers Agent Skills.
2. Keep the full review workflow in `SKILL.md`; do not copy it into `AGENTS.md`.
3. If automatic end-of-task review is desired, add only a small project instruction telling Codex to invoke `compounding-review` before the final response for substantive tasks. This is routing, not the review logic itself.

Suggested project instruction:

> At the end of substantive coding, debugging, research, design, or delegated multi-agent work, use the `compounding-review` skill before the final response. Skip or keep it minimal for trivial edits. The review must separate AI feedback/verification from human-compounding insights.

## Limitation

A skill is model-selected reusable instruction, not a guaranteed lifecycle hook. If the environment must guarantee execution after every qualifying task, enforce that in the surrounding runtime/orchestrator rather than relying only on skill discovery.
