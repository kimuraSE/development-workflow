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

### Per-ViewModel Implementation Steps (mandatory — no skipping)

1. Extract screen states from `@docs/screen.md`
2. Define the ViewModel state representation
3. Extract events from `@docs/screen.md`
4. Implement event handlers
5. Implement state transitions
6. Connect UseCase calls
7. Enter plan mode and define a test plan for state transitions
8. Present the test plan to the user and wait for approval

### SubAgent Usage ①: During test plan review (before step 8)
→ Launch `test-writer-fixer` to verify:
  - Whether all states and transitions in `@docs/screen.md` are covered by test cases
  - Whether the mock strategy for UseCase connections is appropriate (ViewModels not depending on UseCase implementations)
  - Whether test cases exist for error states and loading states
→ Present the review results to the user before proceeding with implementation.

9. Implement and execute tests only after approval
10. Present test results and logs as verification evidence

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

## Definition of Done (per ViewModel)

Mark as "Done" only when ALL of the following are satisfied:

- [ ] All states strictly match `@docs/screen.md`
- [ ] All events and transitions are implemented and tested
- [ ] All UseCase connections are verified
- [ ] No undefined states or transitions exist
- [ ] No temporary fixes, hacks, or workarounds exist
- [ ] Architectural integrity is preserved (no dependency direction violations)
- [ ] All tests pass and results are presented as evidence
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