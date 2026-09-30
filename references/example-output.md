# Example output

### AI REVIEW

**Outcome**
Partial. The implementation passed the targeted test, but no regression suite was run, so broader correctness remains unverified.

**Execution review**
- OBSERVED: the first implementation assumed provider timeout would transition the persisted job state; the next observation showed the job could remain PENDING.
- OBSERVED: inspecting stale recovery changed the diagnosis and led to the final patch.
- CHANGE: inspect all state-transition mechanisms before editing retry behavior; the initial narrow model caused an avoidable implementation loop.

**Expectation -> observation**
- Expected: provider timeout would make the job terminal.
- Observed: request waiting ended, but persisted state remained non-terminal.
- Meaning: request lifecycle and persisted-state lifecycle must be analyzed separately.

**Feedback for the next run**
- KEEP: use an independent verification step after state-machine changes.
- CHANGE: enumerate terminal, non-terminal, and unknown paths before implementation.
- TRY: add a failure-path table to the plan for asynchronous state-machine work.
- VERIFY: run the broader regression suite.

### HUMAN COMPOUNDING

**What is worth understanding**
Timeout and stale recovery solve different lifecycle problems: one stops waiting for a request; the other repairs persisted state. One does not imply the other.

**Why it matters**
Treating them as one mechanism can make debugging focus on provider behavior while the real failure is in state recovery.

**New model**
For asynchronous jobs, reason separately about request lifecycle, persistence lifecycle, and recovery lifecycle.

**Transfer rule**
When you see a job stuck in an intermediate state -> consider lifecycle mismatch -> check timeout, transaction/status transition, and stale recovery before changing retry logic.

**Reusable asset candidate**
A state-machine failure-path checklist is likely more reusable than a prose summary of this bug.

**Still uncertain**
Whether the same pattern applies to synchronous jobs was not tested.
