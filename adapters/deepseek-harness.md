# DeepSeek Harness adapter

## Goal

Use the same `compounding-review` skill through DeepSeek Harness's skill subsystem, while optionally using a Harness plugin to guarantee post-task invocation and provide richer trajectory evidence.

## v0 — skill-only

Expose the skill through the Harness filesystem skill provider / skill catalog. Invoke it explicitly or instruct the main agent to load it before completing substantive tasks.

## v1 — runtime integration

Because DeepSeek Harness is plugin-based and exposes live agent/session activity, add a small plugin that:

1. observes a root agent reaching idle/completed state;
2. collects only the review-safe trajectory needed for the task (goal, consequential tool/agent events, observations, verification results, child-agent relations);
3. invokes a review pass using `compounding-review`;
4. appends or surfaces the two outputs separately: `AI REVIEW` and `HUMAN COMPOUNDING`;
5. records whether multiple agents were involved and their explicit parent/child or delegated relationships when available.

Do not infer multi-agent merely from multiple tool calls.

## Important implementation constraint

DeepSeek Harness is currently pre-stable. Keep the adapter thin and isolate event/API-specific code in the plugin. Do not fork or patch the core solely to implement this review workflow.

## Suggested review event schema

```json
{
  "task_id": "...",
  "root_agent_id": "...",
  "status": "complete|partial|failed|unverified",
  "agents": [
    {"id": "...", "parent_id": null, "role": "..."}
  ],
  "consequential_events": [
    {
      "actor": "agent-or-role",
      "kind": "plan|search|tool|handoff|observation|verification|user_feedback",
      "summary": "...",
      "evidence_ref": "..."
    }
  ]
}
```

Keep raw hidden reasoning out of this schema. Capture observable actions, outputs, explicit rationales, and evidence only.
