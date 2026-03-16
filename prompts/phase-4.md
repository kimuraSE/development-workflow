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

## Test Plan Rules (MANDATORY)

Before writing any test code, always do the following:

1. Enter plan mode
2. Define a test plan including:
   - Target UseCase methods and behaviors
   - Test cases and expected results
   - Edge cases and invariant verification
   - Mock strategy for dependent Domain/Repository objects

### SubAgent Usage ①: During test plan review
→ Launch `test-writer-fixer` to verify:
  - Validity of the mock strategy (whether Domain layer dependencies are correctly isolated)
  - Test case coverage (missing boundary values, edge cases, invariant checks)
  - Whether mandatory Legal UseCase test cases are included:
    - `HasAgreedToTermsUseCase`: returns false when no agreement is saved / returns true after saving
    - `SaveAgreementUseCase`: agreement state is correctly persisted / calling again does not overwrite with wrong value
→ Present the review results to the user before proceeding with implementation.

3. Implement and execute tests only after approval
4. Present test results and logs as verification evidence

## Definition of Done (per UseCase)

Mark as "Done" only when ALL of the following are satisfied:

- [ ] Implementation matches `@docs/domain-design.md` and `@docs/requirements.md`
- [ ] Unit tests are implemented and all pass
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