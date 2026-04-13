# Phase 5: Repository (Data) Layer Implementation

Strictly follow all rules in @CLAUDE.md (Workflow Orchestration, Failure Handling,
Scope Control, Definition of Done, Phase Gates).

## Language Rules (MANDATORY)
- Questions and explanations to the user: English
- Code, variable names, and comments: follow conventions defined in `@docs/design.md`

## Preconditions (STRICT)
Confirm all of the following before starting. If anything is incomplete or inconsistent: STOP → ask the user.

- [ ] Phase 4 is complete (all UseCases implemented, all tests pass)
- [ ] Repository interfaces in the Domain layer are finalized
- [ ] `@docs/requirements.md` is approved
- [ ] `@docs/design.md` is approved
- [ ] `@docs/domain-design.md` is approved
- [ ] `@docs/db-schema.md` is approved

## Scope (Phase 5 only)

**Allowed:**
- Repository implementations in the Data layer
- DataSource, DTO, and Mapper within the Data layer boundary
- Unit tests for all Repository methods (in-memory only)
- Preserving domain constraints in data mapping

**Prohibited:**
- UI/View/ViewModel implementation
- Creating files outside the Data layer boundary defined in `@docs/design.md`
- Modifying Repository interfaces, DB schema, or the Domain/UseCase layer without following the change rules below

## Legal Repository Identification (MANDATORY)

Before planning the implementation order, review `@docs/requirements.md` Section 4.1:

**If Section 4.1 is "required"** → confirm `AgreementRepository` in the Phase 4 interface list
- If missing: STOP → ask the user
- Implementation must use the storage mechanism defined in `@docs/design.md` (e.g., UserDefaults for iOS, DataStore for Android)
- Provide an in-memory implementation for testing

**If Section 4.1 is "not required"** → explicitly document this as out of scope in the implementation plan before proceeding.

## DB Schema Reference Rules (MANDATORY)
Before implementing each Repository, review `@docs/db-schema.md` and confirm:
- Whether the tables used by this Repository are defined in db-schema.md
- Whether field names, data types, and constraints match
- Whether relationships (foreign keys, etc.) are correctly reflected
- If an undefined schema structure is needed: STOP → follow the DB Schema Change Rules below

## Change Rules

**If DB schema changes are required:**
1. Immediately STOP implementation → explain the change and reason to the user
2. Update `@docs/db-schema.md` → verify consistency with `@docs/domain-design.md`
3. Obtain user approval → resume implementation

**If Repository interface changes are required:**
1. Immediately STOP implementation → explain the change and reason to the user
2. Update `@docs/domain-design.md` → obtain user approval
3. Notify the user that Phase 4 UseCase tests may be affected
4. Apply the minimal interface change
5. Re-run all Phase 3 and 4 tests to confirm no regression → resume implementation

**If Domain/UseCase layer changes are required:**
1. Immediately STOP implementation → explain the change and reason to the user
2. Update the relevant `@docs` file → obtain user approval
3. Apply the minimal fix
4. Re-run all Phase 3 and 4 tests to confirm no regression → resume implementation

## DTO / Mapper Rules
- Place DTOs and Mappers within the Data layer boundary defined in `@docs/design.md`
- Mappers convert bidirectionally between Domain objects and DTOs without violating domain constraints
- DTO structure must conform to the table definitions in `@docs/db-schema.md`
- Modifying Domain objects to accommodate DTO structure is prohibited
- If a mapping conflicts with domain constraints: STOP → explain to the user

## Implementation Order Rules

Plan the implementation order in plan mode before starting, present it to the user, and obtain approval.
Do not begin implementation before approval.

Priority:
1. Start with Repositories that have the fewest dependencies
2. Prioritize Repositories that are used most frequently by UseCases
3. Before implementing each Repository, review the corresponding interface, DTO mapping, and table definitions in `@docs/db-schema.md`
4. Include `AgreementRepository` in the plan at the same level as regular Repositories

The approved order defines the queue for the Per-Function Cycle below. One function at a time, no batching.

## Per-Function Cycle (Strict)

This phase MUST follow the Per-Function Implementation Cadence in @CLAUDE.md Section 4-bis.

For each function (public Repository method) in the approved order, complete ALL 5 steps before starting the next function:

1. Implement ONE function only.
2. STOP → present implementation (file, code, intent, expected behavior, DTO mapping notes) → wait for explicit user approval.
3. Present a Test Plan scoped to THIS function only (see Test Plan Rules) → wait for explicit user approval.
4. Implement & run the test (in-memory) → present results/logs → wait for explicit user approval.
5. Move to the next function.

DTOs and Mappers introduced solely to support a Repository method are bundled into that method's cycle. Standalone DTOs/Mappers shared across multiple Repositories may be proposed as a single review-only item (no test) and require explicit approval.

Implementing 2+ functions before stopping, or batching test plans across functions, is a Failure (Section 7).

## Test Plan Rules (per function)

Each test plan in step 3 of the cycle must include:
- Target Repository method and its expected behaviors
- Test cases and expected results using in-memory storage
- Edge cases and domain constraint verification
- DTO mapping accuracy verification (when this method touches mapping)
- Verification that field constraints in `@docs/db-schema.md` (NOT NULL, UNIQUE, etc.) relevant to this method are correctly reflected

### SubAgent Usage ①: During each per-function test plan review
→ Launch `test-writer-fixer` to verify:
  - Whether DTO mapping tests cover both directions (Domain→DTO and DTO→Domain) for THIS function
  - Whether test cases exist for the constraints in db-schema.md that apply to THIS function (NOT NULL, UNIQUE, DEFAULT, etc.)
  - For `AgreementRepository` functions (if in scope), whether mandatory cases are included:
    - Agreement state can be correctly saved and retrieved
    - Returns false (or equivalent default) when no agreement has been saved
    - Saving twice does not corrupt the stored value
    - In-memory implementation satisfies the same interface as the production implementation
→ Present the review results to the user before requesting test plan approval.

## Definition of Done (per function)

Mark a function "Done" only when ALL of the following are satisfied:

- [ ] Implementation conforms to the Repository interface defined in the Domain layer
- [ ] Implementation conforms to the table definitions in `@docs/db-schema.md`
- [ ] User has explicitly approved the implementation (cycle step 2)
- [ ] User has explicitly approved the test plan (cycle step 3)
- [ ] In-memory unit tests are implemented and all pass
- [ ] User has explicitly approved the test results (cycle step 4)
- [ ] Data mapping preserves all domain constraints
- [ ] No temporary fixes, hacks, or workarounds exist
- [ ] Architectural integrity is preserved (no dependency direction violations)
- [ ] Test results and logs are presented as evidence

## Exit Criteria (Phase 5)

### SubAgent Usage ②: During Exit Criteria verification
→ Launch `backend-architect` to verify:
  - Whether all Repository implementations conform to the Domain layer interfaces
  - Whether the dependency direction from the Data layer to the Domain/UseCase layer is correct
  - Whether DTOs are contained within the Data layer boundary
  - Whether Phase 3 and 4 tests have any regressions
→ If issues are found, present them, resolve them, and then do a final confirmation.

Mark Phase 5 as complete only when ALL of the following are satisfied:

- [ ] All Repository implementations conform to Domain interfaces
- [ ] All Repository implementations conform to `@docs/db-schema.md`
- [ ] All Repository unit tests (in-memory) pass
- [ ] Data mapping preserves domain constraints
- [ ] Phase 3 (Domain) and Phase 4 (UseCase) tests all pass (no regression)
- [ ] Zero failing tests
- [ ] Definition of Done conditions are met for all Repositories
- [ ] `AgreementRepository` is implemented and tested (or explicitly recorded as out of scope)
- [ ] `backend-architect` Data layer architecture consistency check completed

## Start
First review `@docs/requirements.md` Section 4.1 to determine whether `AgreementRepository` is required,
then list all defined Repository interfaces (including Legal Repositories) based on
`@docs/design.md`, `@docs/domain-design.md`, and `@docs/db-schema.md`,
create an implementation order plan in plan mode, and propose it to the user.
Obtain user approval before starting implementation.