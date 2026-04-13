# Phase 3: Domain Layer Implementation

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Code, variable names, and comments: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is unapproved or inconsistent: STOP → ask the user.

- [ ] `@docs/requirements.md` is approved
- [ ] `@docs/design.md` is approved
- [ ] `@docs/domain-design.md` is approved

## Scope (Phase 3 only)

**Allowed:**
- Domain layer (Entities, value objects, aggregates, Domain services, domain rules/invariants)
- Unit tests for all Domain methods

**Prohibited:**
- Implementing UseCase, Data/Repository, UI/View, or ViewModel
- Creating files outside the Domain layer boundary defined in `@docs/design.md`
- Adding requirements or redesigning specs (if deemed necessary: STOP → ask the user)

## Implementation Order Rules

Before starting implementation, plan the order in plan mode and present it to the user for approval.
Do not begin implementation before approval.

Priority:
1. Start with value objects that have the fewest dependencies
2. Follow the order: value objects → Entities → aggregates → Domain services
3. If dependencies exist, implement dependencies first

The approved order defines the queue for the Per-Function Cycle below. One function at a time, no batching.

## Per-Function Cycle (Strict)

This phase MUST follow the Per-Function Implementation Cadence in @CLAUDE.md Section 4-bis.

For each function (public method) in the approved order, complete ALL 5 steps before starting the next function:

1. Implement ONE function only.
2. STOP → present implementation (file, code, intent, expected behavior) → wait for explicit user approval.
3. Present a Test Plan scoped to THIS function only (see Test Plan Rules) → wait for explicit user approval.
4. Implement & run the test → present results/logs → wait for explicit user approval.
5. Move to the next function.

Implementing 2+ functions before stopping, or batching test plans across functions, is a Failure (Section 7).

## Test Plan Rules (per function)

Each test plan in step 3 of the cycle must include:
- Target method and its expected behaviors
- Test cases and expected results
- Edge cases and invariant verification

### SubAgent Usage ①: During each per-function test plan review
→ Launch `test-writer-fixer` to verify:
  - Test case coverage for THIS function (missing boundary values, edge cases, invariant checks)
  - Whether the domain rules in `@docs/domain-design.md` that apply to THIS function are covered
→ Present the review results to the user before requesting test plan approval.

## Definition of Done (per function)

Mark a function "Done" only when ALL of the following are satisfied:

- [ ] Implementation matches `@docs/domain-design.md`
- [ ] User has explicitly approved the implementation (cycle step 2)
- [ ] User has explicitly approved the test plan (cycle step 3)
- [ ] Unit tests are implemented and all pass
- [ ] User has explicitly approved the test results (cycle step 4)
- [ ] No temporary fixes, hacks, or workarounds exist
- [ ] Architectural integrity is preserved (no dependency direction violations)
- [ ] Test results and logs are presented as evidence

## Exit Criteria (Phase 3)

### SubAgent Usage ②: During Exit Criteria verification
→ Launch `backend-architect` to verify:
  - Whether Clean Architecture dependency direction is followed throughout the entire Domain layer
  - Whether all Entities, value objects, and aggregates defined in `@docs/domain-design.md` are implemented
  - Whether there are any dependency leaks from the Domain layer to outer layers
→ If issues are found, present them, resolve them, and then do a final confirmation.

Mark Phase 3 as complete only when ALL of the following are satisfied:

- [ ] All Domain methods are implemented
- [ ] All unit tests pass
- [ ] Behavior matches `@docs/domain-design.md`
- [ ] Zero failing tests
- [ ] Definition of Done conditions are met for all methods
- [ ] `backend-architect` architecture consistency check completed

## Start
Review the content of `@docs/domain-design.md`,
create an implementation order plan in plan mode, and propose it to the user.
Obtain user approval before starting implementation.