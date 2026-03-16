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

## Test Plan Rules (MANDATORY)

Before writing any test code, always do the following:

1. Enter plan mode
2. Define a test plan including:
   - Target methods and behaviors
   - Test cases and expected results
   - Edge cases and invariant verification
3. Present the test plan to the user
4. Wait for explicit approval

### SubAgent Usage ①: During test plan review
→ Launch `test-writer-fixer` to verify:
  - Test case coverage (missing boundary values, edge cases, invariant checks)
  - Whether all domain rules in `@docs/domain-design.md` are covered by tests
→ Present the review results to the user before proceeding with implementation.

5. Implement and execute tests only after approval
6. Present test results and logs as verification evidence

## Definition of Done (per method)

Mark as "Done" only when ALL of the following are satisfied:

- [ ] Implementation matches `@docs/domain-design.md`
- [ ] Unit tests are implemented and all pass
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