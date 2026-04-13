# Lessons Log

### [2026-04-13] Workflow: Implementation Cadence
- **What happened**: During implementation phases (Phase 3/4/5/8), the assistant implemented many functions in a single autonomous run before stopping for review. The user reported feeling "left behind" — unable to keep up with what was being built or to course-correct early.
- **Root cause**: The phase prompts and CLAUDE.md only required user approval at the layer/phase level (one Test Plan covering many methods). Per-function review was not enforced, so the assistant batched work to maximize throughput.
- **Rule to prevent recurrence**: Always follow the Per-Function Implementation Cadence in @CLAUDE.md Section 4-bis for Phase 3/4/5/8. One public method = one full cycle (implement → approve → test plan → approve → test → approve) before moving to the next. Layer-wide batching of implementation or testing is a Failure (Section 7). Silence is NOT approval — wait for explicit "OK"/"承認"/"進めて" each time.
