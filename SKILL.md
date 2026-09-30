---
name: compounding-review
description: Review a completed AI-agent task or interaction. Produce (1) evidence-grounded feedback and verification for the AI/agents and (2) concise, transferable learning for the human. Use at the end of substantive coding, research, debugging, design, or multi-agent work; also use when the user asks what happened, why an agent acted as it did, what was learned, or what should be improved next time. Do not force lessons from trivial work.
---

# Compounding Review

## Purpose

After substantive agent work, turn the completed interaction into two different outputs:

1. **AI Review** — feedback, verification, and next-run improvement for the agent(s).
2. **Human Compounding** — only the concepts, mental models, decision rules, failure patterns, and reusable assets worth carrying forward for the human.

These outputs have different audiences. Never collapse them into one generic summary.

## Core rule

Do not summarize the transcript. Reconstruct the smallest useful model of what happened.

Prefer evidence from the actual trajectory: user request, plans, tool calls, observations, test results, diffs, evaluator output, and explicit agent statements. Do not invent hidden reasoning.

For every important explanation distinguish:

- **OBSERVED** — directly supported by the trajectory or artifacts.
- **STATED** — rationale explicitly stated by an agent/user at the time.
- **INFERRED** — a post-hoc interpretation. Label it as inference.
- **UNKNOWN** — evidence is insufficient.

Never convert INFERRED into OBSERVED.

## When to run

Run after a substantive unit of work is complete or has clearly stopped. Typical triggers:

- implementation or debugging finished;
- research produced a conclusion;
- an architectural/product decision was made;
- an evaluator/test changed the solution;
- an approach failed in an informative way;
- two or more agents/roles collaborated;
- the user explicitly requests review, learning, feedback, or a post-task recap.

For trivial changes with no meaningful process lesson, keep the review minimal. `Human Compounding: none` is valid.

## Step 1 — Reconstruct the task contract

Identify:

- the user's actual goal;
- success criteria, explicit or reasonably observable;
- important constraints;
- what the agent expected its chosen approach to accomplish.

Do not silently invent acceptance criteria. Mark missing criteria as UNKNOWN.

## Step 2 — Reconstruct the decision/observation chain

Compress the trajectory into only consequential transitions:

`intent -> action/search -> observation -> changed belief/decision -> action -> verification`

Ignore routine tool calls unless they changed the direction of work.

For each transition ask:

- What new evidence appeared?
- Did it change the plan?
- Was a prior assumption corrected?
- Did verification expose a new failure?

## Step 3 — Detect multi-agent work

Treat work as multi-agent when the trajectory contains two or more distinct agent identities, child agents, subagents, delegated model roles, or independent reviewer/evaluator agents.

Do **not** call ordinary tool use multi-agent.

If multi-agent work occurred, reconstruct:

- role of each agent;
- what information/artifact was handed off;
- what incremental information or action each agent added;
- whether role separation was useful, redundant, or unresolved;
- where independent verification changed the result.

Do not assume multi-agent is better. If the second agent added no identifiable value, say so.

## Step 4 — Produce AI Review

The AI-facing section is feedback and verification, not long-term memory.

Use this structure, omitting empty subsections:

### AI REVIEW

**Outcome**
- State whether the task is complete, partial, failed, or unverified.
- Cite concrete evidence available in the trajectory: tests, outputs, diffs, user confirmation, evaluator results.

**Execution review**
- What worked and why, based on evidence.
- What caused avoidable loops, premature assumptions, unnecessary work, weak search, poor handoffs, or missing verification.

**Expectation -> observation**
- Expected effect of the important action/strategy.
- Actual observation.
- Meaningful gap, if any.

**Multi-agent review** (only if applicable)
For each consequential handoff:
`Agent/role A -> artifact/information -> Agent/role B -> incremental result`
Then state whether the split added observable value.

**Feedback for the next run**
- `KEEP`: behavior with evidence of value.
- `CHANGE`: behavior that should change, with reason.
- `TRY`: a testable improvement, not a new dogma.
- `VERIFY`: unresolved claims or outcomes that still need evidence.

Avoid generic praise. Every KEEP/CHANGE item should connect to an observed event.

## Step 5 — Produce Human Compounding

The human-facing section is not an execution report. Select only what is worth carrying into future work.

Use this structure:

### HUMAN COMPOUNDING

**What is worth understanding**
Explain the 1–3 mechanisms, distinctions, or decisions that matter beyond this task. Prefer causal/system explanations over file-by-file narration.

**Why it matters**
Explain why this knowledge changes future diagnosis, design, search, verification, or decision-making.

**Model update**
When a meaningful correction occurred:
- `Before:` the prior working model/assumption, only if supported by the trajectory.
- `After:` the more accurate model supported by current evidence.

Do not fabricate what the user previously believed. If no prior belief is observable, say `New model:` instead.

**Transfer rule**
Whenever possible express a reusable cue:
`When you see X -> consider Y -> check Z before acting.`

**Reusable asset candidate**
Only when justified, suggest one concrete artifact that would reduce future rediscovery cost, such as:
- decision rule;
- debugging checklist;
- architecture pattern;
- eval/test case;
- workflow/agent-collaboration pattern;
- script/automation;
- project rule or documentation update.

Do not create an asset merely because the task ended.

**Still uncertain**
List important interpretations that remain hypotheses, environment-specific, or insufficiently verified.

## Step 6 — Quality gate

Before returning the review, check:

1. Did I separate AI feedback from human learning?
2. Did I explain important expectation-vs-observation gaps?
3. If multiple agents existed, did I explain their actual information flow and incremental value?
4. Did I distinguish observed/stated/inferred/unknown claims where ambiguity matters?
5. Did I avoid inventing hidden chain-of-thought or motives?
6. Is the human section transferable rather than a transcript summary?
7. Did I allow "nothing worth沉淀" when appropriate?
8. Are suggested improvements testable rather than absolute rules?

If evidence is missing, shorten the review rather than filling gaps with plausible stories.

## Output length

Default to concise: enough to preserve the important causal/decision structure, usually 300–800 Chinese characters for routine substantive work. Use more detail for complex multi-agent work or when the user asks for a deep review.

Match the user's language.
