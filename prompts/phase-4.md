# Phase 4: UseCase Layer Implementation

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Code, variable names, and comments: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 3 is complete (all Domain methods implemented, all tests pass)
- [ ] `@docs/requirements.md` is approved
- [ ] `@docs/design.md` is approved
- [ ] `@docs/domain-design.md` is approved

## Scope (Phase 4 only)

**Allowed:**
- UseCase implementations
- Repository interface definition and refinement in the Domain layer (Domain layer only)
- Unit tests for all UseCase methods

**Prohibited:**
- Data layer Repository implementations (Phase 5)
- UI/View/ViewModel implementation
- Creating files outside the layer boundaries defined in `@docs/design.md`
- Modifying the Domain layer without first updating `@docs/domain-design.md`

## Legal UseCase Identification (MANDATORY)

Before planning the implementation order, review `@docs/requirements.md` Section 4.1 and determine:

**If Section 4.1 is "required"** → include the following in the implementation plan (STOP if missing):
- `HasAgreedToTermsUseCase`: returns whether the user has already agreed
- `SaveAgreementUseCase`: persists the user's agreement state

**If Section 4.1 is "not required"** → explicitly document this as out of scope before proceeding.

## Implementation Order Rules

Plan the implementation order in plan mode before starting, present it to the user, and obtain approval.
Do not begin implementation before approval.

Priority:
1. Start with UseCases whose dependent Domain objects are all implemented
2. Prioritize UseCases with high independence (not dependent on other UseCases)
3. For UseCases that require Repository interfaces, define the interfaces first
4. Include Legal UseCases in the plan at the same level as regular UseCases

The approved order defines the queue for the Per-Function Cycle below. One function at a time, no batching.

## Per-Function Cycle (Strict)

This phase MUST follow the Per-Function Implementation Cadence in @CLAUDE.md Section 4-bis.

For each function (public UseCase method) in the approved order, complete ALL 5 steps before starting the next function:

1. Implement ONE function only.
2. STOP → present implementation (file, code, intent, expected behavior, mock dependencies if any) → wait for explicit user approval.
3. Present a Test Plan scoped to THIS function only (see Test Plan Rules) → wait for explicit user approval.
4. Implement & run the test → present results/logs → wait for explicit user approval.
5. Move to the next function.

Repository interface definitions are treated as a separate cycle item (interface → approve → next). They have no test step but still require explicit approval before any UseCase that uses them is implemented.

Implementing 2+ functions before stopping, or batching test plans across functions, is a Failure (Section 7).

## Repository Interface Rules
- Define Repository interfaces before implementing the UseCases that require them
- After definition, verify consistency with `@docs/domain-design.md`
- Design interface definitions in plan mode and obtain user approval before implementing
- Place interfaces in the Domain layer only (Data layer implementations are Phase 5)

## If Domain Layer Changes Are Required
1. Immediately STOP UseCase implementation
2. Explain the change and the reason to the user
3. Update `@docs/domain-design.md` first
4. Obtain user approval
5. Apply the minimal Domain fix
6. Re-run all Phase 3 tests to confirm no regression
7. Resume UseCase implementation

## Test Plan Rules (per function)

Each test plan in step 3 of the cycle must include:
- Target UseCase method and its expected behaviors
- Test cases and expected results
- Edge cases and invariant verification
- Mock strategy for dependent Domain/Repository objects

### SubAgent Usage ①: During each per-function test plan review
→ Launch `test-writer-fixer` to verify:
  - Validity of the mock strategy for THIS function (whether Domain layer dependencies are correctly isolated)
  - Test case coverage for THIS function (missing boundary values, edge cases, invariant checks)
  - For Legal UseCase functions, whether mandatory cases are included:
    - `HasAgreedToTermsUseCase`: returns false when no agreement is saved / returns true after saving
    - `SaveAgreementUseCase`: agreement state is correctly persisted / calling again does not overwrite with wrong value
→ Present the review results to the user before requesting test plan approval.

## Definition of Done (per function)

Mark a function "Done" only when ALL of the following are satisfied:

- [ ] Implementation matches `@docs/domain-design.md` and `@docs/requirements.md`
- [ ] User has explicitly approved the implementation (cycle step 2)
- [ ] User has explicitly approved the test plan (cycle step 3)
- [ ] Unit tests are implemented and all pass
- [ ] User has explicitly approved the test results (cycle step 4)
- [ ] No temporary fixes, hacks, or workarounds exist
- [ ] Architectural integrity is preserved (no dependency direction violations)
- [ ] Repository interfaces match `@docs/domain-design.md`
- [ ] Test results and logs are presented as evidence

## Exit Criteria (Phase 4)

### SubAgent Usage ②: During Exit Criteria verification
→ Launch `backend-architect` to verify:
  - Whether all Repository interfaces are correctly placed in the Domain layer
  - Whether there are any dependency leaks from UseCases to the Data layer
  - Whether Phase 3 (Domain) tests have any regressions
→ If issues are found, present them, resolve them, and then do a final confirmation.

Mark Phase 4 as complete only when ALL of the following are satisfied:

- [ ] All UseCase methods are implemented
- [ ] All UseCase unit tests pass
- [ ] Repository interfaces are defined and match `@docs/domain-design.md`
- [ ] Phase 3 (Domain) tests all pass (no regression)
- [ ] Zero failing tests
- [ ] Definition of Done conditions are met for all UseCases
- [ ] Legal UseCases are implemented (or explicitly recorded as out of scope)
- [ ] `backend-architect` architecture consistency check completed

## Start
First review `@docs/requirements.md` Section 4.1 to determine whether Legal UseCases are required,
then identify the UseCases that need Repository interfaces based on `@docs/domain-design.md`,
create an implementation order plan for all UseCases in plan mode, and propose it to the user.
Obtain user approval before starting implementation.