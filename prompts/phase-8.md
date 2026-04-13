# Phase 8: ViewModel Implementation

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Code, variable names, and comments: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 7 is complete and user-approved
- [ ] Figma design has been verified against `@docs/screen.md` via Figma MCP
- [ ] `@docs/screen.md` is complete and finalized
- [ ] `@docs/requirements.md` is finalized

Re-interpreting or extending specifications is prohibited.

## Scope (Phase 8 only)

**Allowed:**
- ViewModel implementation
- State management logic implementation
- Event handling logic implementation
- Connecting ViewModels to UseCase interfaces (public API only)

**Prohibited:**
- Adding new states not defined in `@docs/screen.md`
- Changing state transitions
- Implementing the View layer or UI logic
- Modifying UseCase or Domain layer
- Introducing "convenience" states not specified in the specs
- Creating files outside the ViewModel layer boundary defined in `@docs/design.md`

## Derivation Rule (Critical)
ViewModel structure MUST be derived from `@docs/screen.md` in the following order:

1. States defined in `@docs/screen.md`
2. Events defined in `@docs/screen.md`
3. Transitions defined in `@docs/screen.md`
4. UseCase mappings defined in `@docs/screen.md`

If a ViewModel element cannot be traced back to `@docs/screen.md`: STOP immediately → ask the user.

## Implementation Order Rules

Before starting, list all ViewModels based on `@docs/screen.md`,
plan the implementation order in plan mode, and present it to the user for approval.
Do not begin implementation before approval.

For each ViewModel in the approved order, extract from `@docs/screen.md`:
- The state representation (treated as one cycle item — review-only, no test)
- The list of public functions: each event handler, each transition method, each UseCase-connecting call

Present this per-ViewModel function list to the user and obtain approval before entering the Per-Function Cycle. The approved list defines the queue.

## Per-Function Cycle (Strict)

This phase MUST follow the Per-Function Implementation Cadence in @CLAUDE.md Section 4-bis.

For each function (public ViewModel method / event handler) in the approved per-ViewModel list, complete ALL 5 steps before starting the next function:

1. Implement ONE function only (one event handler, or one transition method, or one UseCase-connecting call).
2. STOP → present implementation (file, code, the screen.md state/event it derives from, expected transition) → wait for explicit user approval.
3. Present a Test Plan scoped to THIS function only (see Test Plan Rules) → wait for explicit user approval.
4. Implement & run the test → present results/logs → wait for explicit user approval.
5. Move to the next function.

The state representation (data class / sealed class / equivalent) MAY be a single review-only cycle item per ViewModel (no test step required) when it contains only declarations, but transitions on those states are individual functions and DO require the full cycle.

Implementing 2+ functions before stopping, or batching test plans across functions, is a Failure (Section 7).

## Test Plan Rules (per function)

Each test plan in step 3 of the cycle must include:
- Target ViewModel method and its expected state transition(s)
- Test cases mapped 1:1 to the events/transitions in `@docs/screen.md` for THIS function
- Mock strategy for UseCase dependencies
- Loading and error state coverage when applicable

### SubAgent Usage ①: During each per-function test plan review
→ Launch `test-writer-fixer` to verify:
  - Whether the states and transitions in `@docs/screen.md` that THIS function is responsible for are fully covered
  - Whether the mock strategy for UseCase connections is appropriate (ViewModel not depending on UseCase implementations)
  - Whether error and loading state cases are included when applicable
→ Present the review results to the user before requesting test plan approval.

## If screen.md Changes Are Required
1. Immediately STOP ViewModel implementation
2. Explain the gap or inconsistency to the user
3. Update `@docs/screen.md`
4. Obtain user approval
5. Resume ViewModel implementation from the affected point

## Regression Rule
After each ViewModel implementation is complete:
- Re-run all Phase 3 (Domain), Phase 4 (UseCase), and Phase 5 (Repository) tests
- Confirm no regression has occurred
- Present test results as evidence before marking as "Done"

## Definition of Done (per function)

Mark a function "Done" only when ALL of the following are satisfied:

- [ ] Implementation strictly matches the state/event/transition in `@docs/screen.md`
- [ ] User has explicitly approved the implementation (cycle step 2)
- [ ] User has explicitly approved the test plan (cycle step 3)
- [ ] Tests for THIS function are implemented and pass
- [ ] User has explicitly approved the test results (cycle step 4)
- [ ] No undefined states or transitions are introduced
- [ ] No temporary fixes, hacks, or workarounds exist
- [ ] Architectural integrity is preserved (no dependency direction violations)

## Definition of Done (per ViewModel)

Mark a ViewModel "Done" only when ALL of the following are satisfied:

- [ ] Every function in the approved per-ViewModel list has met the per-function Definition of Done
- [ ] All states strictly match `@docs/screen.md`
- [ ] All events and transitions are implemented and tested
- [ ] All UseCase connections are verified
- [ ] Phase 3, 4, and 5 tests have no regressions

## Exit Criteria (Phase 8)

### SubAgent Usage ②: During Exit Criteria verification
→ Launch `frontend-developer` to verify:
  - Whether all ViewModels strictly match the state definitions in `@docs/screen.md`
  - Whether there are any dependency leaks from the ViewModel layer to the View layer
  - Whether all ViewModel UseCase connections use the correct interfaces
  - Whether Phase 3, 4, and 5 tests have any regressions
→ If issues are found, present them, resolve them, and then do a final confirmation.

Mark Phase 8 as complete only when ALL of the following are satisfied:

- [ ] All ViewModels strictly match `@docs/screen.md`
- [ ] All state transition tests pass
- [ ] No inconsistent state behavior exists
- [ ] Phase 3, 4, and 5 tests all pass (no regression)
- [ ] Definition of Done conditions are met for all ViewModels
- [ ] `test-writer-fixer` test coverage check completed
- [ ] `frontend-developer` ViewModel design quality check completed
- [ ] User has confirmed completion

## Start
Review the content of `@docs/screen.md`,
list all ViewModels that need to be implemented,
plan the implementation order in plan mode, and propose it to the user.
Obtain user approval before starting implementation.